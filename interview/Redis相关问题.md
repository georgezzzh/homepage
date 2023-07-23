# Redis其他面试问题

### Redis生成分布式id

使用incr命令，incr具备incr and get原子操作，增加并返回结果的原子操作。

Redis是单进程单线程架构，incr命令不会出现id重复。

```java
public class IDGenerator {
    @Autowired
    private StringRedisTemplate template;
    private static final String ID_KEY = "id:generator:product";
    //全局id生成器
    public long incrementId(){
        long n = this.template.opsForValue().increment(ID_KEY);
        return n;
    }
}
```

### Redis生成分布式锁

由于分布式系统多线程分布在不同机器上，单纯的Java API不能提供分布式锁的能力。

跨JVM的互斥机制控制共享资源的访问。

1. 使用setnx设置锁

使用del来释放锁

```java
# 设置锁
setnx lock 1
# 释放锁
del lock
```

为了防止超时不释放锁，或者线程出错无法释放锁，给锁加一个过期时间

```java
# 10s的过期时间，到期自动释放
expire lock 10
```

为了保证设置过期时间和上锁的原子性，使用set命令

```java
# nx表示不存在创建，ex表示过期时间12s
set users 10 nx ex 12
```

**使用UUID防止释放了别的线程的锁**

如果在释放锁的时候不加以判断，可能会导致释放了其他线程的锁。

线程A：

1. 上锁
2. 执行业务
3. 服务器卡顿了
4. 锁到期释放
5. 服务器反应过来，进行操作手动释放锁。

线程B：

1. 上锁
2. 具体操作
3. 线程B的锁被A释放掉了

改良：在上锁的时候，value设置成自己的uuid，在释放的时候判断该锁是否是自己的锁。

```java
public String redisLock(){
        //获取锁，
        String uuid = UUID.randomUUID().toString();
				//设置锁10s过期
        Boolean lock = stringRedisTemplate.opsForValue().setIfAbsent("lock", uuid,10, TimeUnit.SECONDS);
				// 如果获取成功
        if(lock){
            //业务流程
            System.out.println("获取锁成功");
            String num = stringRedisTemplate.opsForValue().get("num");
            int i = Integer.parseInt(num);
            i += 1;
            stringRedisTemplate.opsForValue().set("num", Integer.toString(i));
            //释放锁, 只释放自己的锁
            String lockUUID = stringRedisTemplate.opsForValue().get("lock");
            if(uuid.equals(lockUUID))
                stringRedisTemplate.delete("lock");
        }
}
```

**删除锁用原子性操作**

如果删除时候不用原子性操作，可能线程A比较UUID发现一致，正要删除时，锁已经过期自动释放了，此时线程B抢到了锁，而线程A删除的锁是线程B的锁。

解决方案，使用lua脚本来执行原子化删除操作。

```java
@RequestMapping("/redis")
    public String redisLock(){
        //获取锁，
        String uuid = UUID.randomUUID().toString();
        Boolean lock = stringRedisTemplate.opsForValue().setIfAbsent("lock", uuid,10, TimeUnit.SECONDS);
        if(lock){
            //业务流程
            System.out.println("获取锁成功");
            String num = stringRedisTemplate.opsForValue().get("num");
            int i = Integer.parseInt(num);
            i += 1;
            stringRedisTemplate.opsForValue().set("num", Integer.toString(i));
            //释放锁, 只释放自己的锁
            //使用lua脚本原子化 删除锁
            String luaScript =
                    "if redis.call('get', KEYS[1])==ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
            DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
            redisScript.setScriptText(luaScript);
            redisScript.setResultType(Long.class);
            //第一个参数是脚本，第二个参数是key，第三个参数是value
            stringRedisTemplate.execute(redisScript, List.of("lock"), uuid);
        }
}
```

### 使用Redission来申请锁

防止redis的锁在未完成任务就把锁提前释放了

Redission使用watch dog机制，在锁到期了，任务没有结束，再进行续期。

```java
public String redissionLock(){
      RLock lock = redissonClient.getLock("lock");
      try {
          //最大等待时间300ms，上锁30ms自动解锁
          if(lock.tryLock(300,30,TimeUnit.MICROSECONDS)){
              System.out.println("加锁成功");
          }
      } catch (InterruptedException e) {
          throw new RuntimeException(e);
      }finally {
          //释放锁, 如果锁由当前线程保留，则释放掉
          if(lock.isHeldByCurrentThread()){
              System.out.println("释放锁");
              lock.unlock();
          }
      }
      return "ok redission";
  }
```

### Redis缓存过期策略

1. **volatile-lru（least recently used）**：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
2. **volatile-ttl**：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
3. **volatile-random**：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
4. **allkeys-lru（least recently used）**：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）
5. **allkeys-random**：从数据集（server.db[i].dict）中任意选择数据淘汰
6. **no-eviction**：禁止驱逐数据，也就是说当内存不足以容纳新写入数据时，新写入操作会报错。这个应该没人使用吧！

### Redis持久化手段

RDB和AOF，Redis默认是rdb

RDB： 可以通过创建快照来获得存储在内存里面的数据在某个时间点上的副本。

AOF： 开启 AOF 持久化后每执行一条会更改 Redis 中的数据的命令，Redis 就会将该命令写入到内存缓存 `server.aof_buf`中

### Redis事务中途出错

以multi开启事务

如果中途出错，那么接下来继续执行剩下的命令。

### 布隆过滤器

布隆过滤器由Bloom提出，使用一个数组表示，其中用3个Hash函数，映射值到数组下标，对应位置置1。

如果该位置是1，则不一定代表数组中没有，因为可能发生Hash冲突。

如果其中有个槽的位置是0，则代表数组中一定不存在该值。

布隆过滤器可以用来解决Redis缓存穿透（penetrate），指的是外部一直访问一个不存在的数据，Redis无法缓存，频繁访问数据库。

### 缓存击穿

大规模的key同一时间失效，缓存相当于没有。

解决措施就是，尽量设置的过期时间随机点儿。

### Redis持久化

Aof和RDB