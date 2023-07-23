### Java util log

#### 输出日志到控制台

```java
Logger logger = Logger.getLogger("com.company");
logger.info("fine msg");
logger.log(Level.INFO,"输出日志");
```

#### 记录日志到文件

```java
//日志
Logger fileLog = Logger.getLogger("com.company");
//日志处理器：文件处理器
FileHandler fileHandler = new FileHandler("/home/java.log"); # 日志路径为 /home/java.log
//设置格式，fileHandler默认为XML格式，我们设置为简单文本格式
fileHandler.setFormatter(new SimpleFormatter());
fileLog.addHandler(fileHandler);
fileLog.info("fileLog");
```

#### 日志配置文件详解

jdk 11中日志配置文件路径，`/opt/jdk/conf/logging.properties`

**logging.properties**内容

```
# 指定的RootLogger， 默认处理器为ConsoleHandler
handlers= java.util.logging.ConsoleHandler
.level= INFO # RootLogger默认的日志级别, INFO

# 日志文件存储的默认路径，%h表示HOME，%u表示数字
java.util.logging.FileHandler.pattern = %h/java%u.log
java.util.logging.FileHandler.limit = 50000 # 指定日志记录为5w条
java.util.logging.FileHandler.count = 1
java.util.logging.FileHandler.maxLocks = 100
java.util.logging.FileHandler.formatter = java.util.logging.XMLFormatter # 日志消息格式

# 指定ConsoleHandler对象的日志级别
java.util.logging.ConsoleHandler.level = ALL
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
# messages:
com.xyz.foo.level = SEVERE
```

java读取日志配置文件

```java
InputStream inS = Main.class.getClassLoader().getResourceAsStream("logging.properties");
//获取日志管理器
LogManager logManager = LogManager.getLogManager();
//日志管理器读取日志配置文件
logManager.readConfiguration(inS);
Logger logger = Logger.getLogger("com.company");
logger.fine("fine msg");
```

**自定义的日志记录器配置**

logging.properties中配置如下

```
# 自定义日志管理器, 日志记录器名为com.company
com.company.handlers = java.util.logging.ConsoleHandler
com.company.level = ALL
com.company.parent = false # 不继承父类配置
```

源代码中获取com.company日志记录器，获取到的日志记录器为上面配置文件中配置的

```java
Logger logger = Logger.getLogger("com.company");
logger.fine("fine msg");
```

#### 日志解析流程

1. 初始化LogManager
   1. LogManager加载logging.properties配置
   2. 添加Logger到LogManager
2. 从LogManager中获取Logger
3. 加载配置Level, Filter, Handler, Formatter初始化Logger







