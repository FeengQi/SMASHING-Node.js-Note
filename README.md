# 了不起的Node js 将JavaScript进行到底

#### PART Ⅰ　从安装与概念开始 
- CHAPTER 1 安装
  - 在window下安装 
  - 在OS X下安装
  - 在Linux下安装
    - 编译 
    - 确保安装成功
  - Node REPL
    - 要运行Node的REPL，在终端输入node即可
  - 执行文件
  - NPM
    - 安装模块
      - `npm install` 
    - 自定义模块
    - 安装二进制包
    - 浏览NPM仓库
      - 搜索模块`npm search xxx`
      - 查看模块的package.json文件`npm view xxx`
  - 小结 
  
- CHAPTER 2 JavaScript概览
  - 介绍
  - JavaScript基础
    - 类型
      - 基本类型：number、boolean、string、null以及undefined（和symbol（es6））
      - 复杂类型：array、function以及object
    - 类型的困惑
      - 判断变量值的类型
        ``` 
        ```
    - 函数
    - THIS、FUNCTION#CALL以及FUNCTION#APPLY 
    - 函数的参数
    - 闭包
    - 类
    - 继承
    - TRY {} CATCH {}
  - v8中的JavaScript
    - OBJECT#KEYS
    - ARRAY#ISARRAY
    - 数组方法
    - 字符串方法
    - JSON
    - FUNCTION#BIND
    - FUNCTION#NAME
    - _PROTO_（继承）
    - 存取器
  - 小结
- CHAPTER 2 阻塞与非阻塞IO
  - 能力越强，责任就越大
    - 阻塞
      - Node.js使用事件轮询，是非阻塞的
        - 采用事件轮询意味着什么呢？从本质上来说，Node会先注册事件，随后不停地询问内核这些事件是否已分发。当事件分发时，对应的回调函数就会被触发，
  然后继续执行下去。如果没有事件触发，则继续执行其他代码，直到有新的事件时，再去执行对应的回调函数。
        - Node并发实现也采用了事件轮询。与timeout所采用的技术一样，所有像http、net这样的原生模块中的IO部分也都采用了事件轮询技术。和timeout机制
        中Node内部会不停地等待，并当超时完成时，触发一个消息通知一样，Node使用事件轮询，触发一个和文件描述符相关的通知。
        - 文件描述符是抽象的句柄，存有对打开的文件、socket、管道等的引用。本质上来说，当Node接收到浏览器发来的HTTP请求时，底层的TCP连接会分配一个
        文件描述符。随后，如果客户端向服务器发送数据，Node就会收到该文件描述符上的通知，然后出发JavaScript的回调函数。
    - 单线程的世界
      - Node是单线程的。在没有第三方模块的帮助下是无法改变这一事实的。
      - JavaScript的timeout并不能严格遵守时钟设置。
      - 既然执行时只有一个线程，也就是说，当一个函数执行时，同一时间不可能有第二个函数也在执行。
    - 错误处理
      - Node应用依托在一个拥有大量共享状态的大进程中。
      - 在一个HTTP请求中，如果某个回调函数发生了错误，整个进程都会遭殃
      ```
      var http = require('http');
      
      http.createServer(function(){
        throw new Error('错误不会被捕捉');
      }).listen(3000);
      ```
      - 因为错误未被捕捉，若访问Web服务器，进程就会崩溃。
      - Node之所以这样处理是因为，在发生未被捕获的错误时，进程的状态就不确定了。之后就可能无法正常工作了，并且如果错误始终不处理的话，就会一直抛出意料之外的错误，这样就很难调试。
    - 堆栈追踪
      - 在JavaScript中，当错误发生时，在错误信息中可以看到一系列的函数调用，这称为堆栈追踪。
  - 小结
- CHAPTER 4　Node中的JavaScript
  - global对象
    - 实用的全局对象
      - setTimeout并非ECMAScript的一部分，但浏览器却仍能将其视作重要的特性来实现。事实上，该函数是无法通过纯JavaScript重写的。
  - 模块系统
    - 绝对和相对模块
      - 绝对模块是指Node通过在其内部node_modules查到的模块，或者Node内置的如fs这样的模块。
        - 可以直接通过名字来require这个模块，无需添加路径名
  - 暴露API
    - 要让模块暴露一个API成为require调用的返回值，就要依靠module和exports这两个全局变量。
    - 在默认情况下，每个模块都会暴露一个空对象。
  - 事件
    - Node.js中的基础API之一就是EventEmitter。
    - 浏览器中负责处理时间相关的DOM API主要包括addEventListener、removeEventListener以及dispatchEvent。它们还用在一系列从window到XMLHTTPRequest等的其他对象上。
     - Node暴露了Event、EmitterAPI，该API上定义了on、emit以及removeListener方法，它以process.EventEmitter形式暴露出来。
     - 事件时Node非阻塞设计的重要体现，Node通常不会直接返回数据（因为这样可能会在等待某个资源的时候发生线程阻塞），二十采用分发事件来传递数据的方法。
  - buffer
  - 小结
#### PART Ⅱ　Node重要的API
- CHAPTER 5　命令行工具（CLI）以及FS API：首个Node应用
  - 需求
  - 编写首个Node程序
    - 创建模块
    - 同步还是异步
    - 理解什么是流（stream）
    - 输入和输出 
    - 重构
    - 用fs进行文件操作
  - 对CLI一探究竟
    - argv
    - 工作目录
    - 环境变量
    - 退出
    - 信号
    - ANSI转义码
  - 对fs一探究竟
    - Stream
    - 监视
  - 小结
- CHAPTER 6　TCP

    `Node HTTP服务器是构建与Node TCP服务器之上的。从编程角度来说，也就是Node中的http.Server继承自net.Server（net是TCP模块）。`
    
  - TCP有哪些特性(TCP的首要特性就是它是面向连接的) 
    - 面向连接的通信和保证顺序的传递
      - TCP协议做基于的IP协议是面向无连接的。
      - IP是基于数据报的传输，这些数据报是独立进行传输的，送达的顺序也是无序的。
      - 使用IP协议意味着数据包到达时是无序的，这些数据包不属于任何数据流或者连接。
      - 使用TCP/IP和服务器建立连接后，是怎样做到让数据包送达时是有序的呢？
        - 要回答这个问题其实就等于在解释为什么会有TCP。当在TCP连接内进行数据传递时，发送的IP数据包含了标识该连接以及数据流顺序的信息。
    - 面向字节
      - TCP对字符以及字符编码是完全无知的。不同的编码会导致传输的字节数不同。
      - TCP对消息格式没有严格的约束，使得TCP有很好的灵活性。
    - 可靠性
      - 由于TCP是基于底层不可靠的服务，因此，它必须基于确认和超时实现一系列的机制来达到可靠性的要求。
      - 当数据发送出去后，发送方就会等待一个确认消息（表示数据包已经收到的简短的确认消息）。如果通过了指定的窗口时间，还未收到确认消息，发送方就会对数据进行重发。
      - 这种机制有效地解决了如网络错误或者网络阻塞这样的不可预期的情况。
    - 流控制
      - 要是两台通信的计算机中，有一台速度远快于另一台的话，会怎么样呢？
        - TCP会通过一种叫`流控制`的方式来确保两点之间传输数据的平衡，
    - 拥堵控制
      - TCP有一种内置的机制能够控制数据包的延迟率及丢包率不糊太高，以此来确保服务的质量（QoS）。
        - 和流控制机制能够避免发送方压垮接收方一样。TCP会通过控制数据包的传输速率来避免拥堵的情况。
    - Telnet
      - Telnet是一个早期的网络协议，旨在提供双向的虚拟终端。他是TCP协议上层的协议。
  - 基于TCP的聊天程序
    - 创建模块
    - 理解NET.SERVER.API
    - 接收连接
    - data事件
      - net.Stream是一个EvenEmitter
      - TCP是面向字节的协议
    - 状态以及记录连接情况
    - 圆满完成此程序
  - 一个IRC客户端程序
    - 创建模块
    - 理解NET#STREAM.API
    - 实现部分IRC协议
    - 测试实际的IRC服务器
  - 小结
- CHAPTER 7　HTTP

   `超文本传输协议，又称为HTTP，是一种Web协议，他是属于TCP上层的协议。`
  - HTTP结构
    - HTTP协议构建在请求和响应的概念之上，对应在Node.js中就是由http.ServerRequest和http.ServerResponse这两个构造器构造出来的对象。
  - 头信息
  - 连接
  - 一个简单的Web服务器
    - 创建模块
    - 输出表单
    - method和URL
    - 数据
    - 整合
    - 让程序更健壮
  - 一个Twitter.Web客户端
    - 创建模块
    - 发送一个简单的HTTP请求
    - 发送数据
    - 获取推文
  - superagent来拯救
  - 使用up重启HTTP服务器
  - 小结
#### PART Ⅲ　Web开发
- CHAPTER 8　Connect
  - 使用HTTP构建一个简单的网站
  - 通过Connect实现一个简单的网站
  - 中间件
    - 书写可重用的中间件
    - static中间件
    - query中间件
    - logger中间件
    - body.parser中间件
      - 使用bodyParser中间件来解析请求的消息体
      - server.use(connect.bodyParser())
    - cookie
    - 会话（session）
    - Redis.session
    - methodOverride中间件
      - 
    - basicAuth中间件
      - basicAuth中间件可以对客户端进行基本身份验证
  - 小结
- CHAPTER 9　Express
  - 一个小型Express应用
    - 创建模块
    - HTML
    - SETUP
    - 定义路由
    - 查询
    - 运行
  - 设置
  - 模板引擎
  - 错误处理
  - 快捷方法
  - 路由
  - 中间件
  - 代码组织策略
  - 小结
- CHAPTER 10　WebSocket
  - Ajax
  - HTML5.WebSocket
    - 每次提到WebSocket的时候，其实是咋讲两部分内容：一部分是浏览器实现的WebScket API 
  - 一个ECHO示例
    - 初始化项目
      -  
    - 建立服务器
    - 建立客户端
    - 运行示例程序
  - 鼠标光标
    - 初始化示例程序
    - 建立服务器
    - 建立客户端
    - 运行示例程序
  - 面临一个挑战
    - 关闭并不意味着断开连接
    - JSON
    - 重连
    - 广播
    - WebSocket属于HTML5：早期浏览器不支持
    - 解决方案
  - 小结 
- CHAPTER 11　Socket.IO
  - 传输 
    - 断开.VS.关闭
    - 事件
    - 命名空间
  - 聊天程序 
    - 初始化程序
    - 构建服务器
    - 构建客户端
    - 事件和广播
    - 消息接收确认
  - 一个轮流做DJ的应用 
    - 扩展聊天应用
    - 集成Grooveshark.API
    - 播放歌曲
  - 小结 
#### PART Ⅳ　数据库
- CHAPTER 12　MongoDB
  - 安装
  - 使用MongoDB：一个用户认证的例子
    - 构建应用程序
    
    - 创建Express.App
    - 连接MongoDB
    - 创建文档
    - 查找文档
    - 身份验证中间件
    - 校验
    - 原子性
    - 安全模式
  - Mongoose介绍
    - 定义模型
    - 定义嵌套的键
    - 定义嵌套文档
    - 构建索引
    - 中间件
    - 探测模型状态
    - 查询
    - 扩展查询
    - 排序
    - 选择
    - 限制
    - 跳过
    - 自动产生键
    - 转换
  - 一个使用Mongoose的例子
    - 构建应用
    - 重构
    - 建立模型
  - 小结
- CHAPTER 13　MySQL
  - node-mysql
    - 初始化项目
    - Express应用
    - 连接MySQL
    - 初始化脚本
    - 创建数据
    - 获取数据
  - sequelize
    - 初始化sequelize
    - 初始化Express应用
    - 连接sequelize
    - 定义模型和同步
    - 创建数据
    - 获取数据
    - 删除数据
    - 完整地完成应用
  - 小结
- CHAPTER 14　Redis
  - 安装Redis
  - Redis查询语言
  - 数据类型
    - 字符串
    - 哈希
    - 列表
    - 数据集
    - 有序数据集
  - Redis和Node
    - 使用node-redis实现一个社交图谱
  - 小结
#### PART Ⅴ　测试
- CHAPTER 15　代码共享
  - 什么样的代码可以共享
  - 书写兼容的JavaScript代码
    - 导出模块
    - 模拟实现ECMA.API
    - 模拟实现Node.API
    - 模拟实现浏览器端API
    - 跨浏览器的继承实现
  - 集成到一起：browserbuild
    - 基础案例
  - 小结
- CHAPTER 16　测试
  - 简单测试
    - 测试目标
    - 测试策略
    - 测试程序
  - expect.js
  - API一览
  - Mocha
    - 测试异步代码
    - BDD风格
    - TDD风格
    - export风格
    - 在浏览器端使用Mocha
  - 小结
