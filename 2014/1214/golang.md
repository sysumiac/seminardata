# 我们曾听说过的 Go
--------------------

19:00 14 Dec 2014
Tags: Golang, Go

舜飞科技
苏创绩
suchuangji@gmail.com
微博: @miraclesu-创绩

## 讲啥

- Go 语言是啥
- 有啥特性
- 咋安装
- Hello World

## Go 不是一款好的编程语言
---------------------------

### 没有泛型

**Java**

```
List<String> list = new ArrayList<String>();
list.add("abc");
// list.add(new Integer(1));  // 编译错误
for (String string : list) {
  System.out.println(string); // 无需任何强制类型转换
}
```

**Go**

```
var list []interface{}
list = append(list, 1) // 编译不会报错
list = append(list, "abc")

for _, value := range list {
  v, ok := value.(string)
  if ok {
    fmt.Printf("%s\n", v)
  }
}
```

### 错误处理机制太原始

**Java**

```
try {
  xxfunction();
  mmfunction();
  ...
} catch {
  handleErr();
}
```

**Go**

```
if err := xxfunction(); err != nil {
  handleErrxx()
}

if err := mmfunction(); err != nil {
  handleErrmm()
}
...
```

### 垃圾回收器（GC）不完善

- **Java**

  分代

- **Go**

  Stop The World!

  1.3 完全精确 

### 槽点列表

- Go 不是 OOP 语言
- Go 还不能用来写操作系统，这也叫系统级语言？
- GUI 库呢？
- 一个好点的 IDE 都没有，写毛程序啊
- 一个 Hello World 程序编译出来都几 M 大！
- 动态连接库都不支持！
- Google 自家有用吗？
- 支持 Android 吗？
- 支持 iOS 吗？
- ...

## 为什么要用 Go
-----------------

### 任性？

### 爽！

------------------

## Go 介绍
-------------------

![gopher](img/gopher.png)

### 特性

- 编译型
- 静态类型
- interfaces
- 内存安全
- GC
- 天生并发支持
- 丰富的标准库
- 好用的工具

### 历史

于 2007 年起于 Google 20% 的“业余项目”

于 2009 年 11 月开源（BSD）

2012 年 3 月份发布 Go 1

### 开发团队

- Ken Thompson
  Unix 、Plan9、UTF-8、B编程语言（C的前身）的创立者之一
- Rob Pike
  Unix操作系统、Plan9操作系统、Limbo编程语言和UTF-8编码的主要设计者
- Robert Griesemer
  曾协助制作Java的HotSpot编译器，和Chrome浏览器的JavaScript引擎V8
- Ian Lance Taylor
  GCC社区的超级活跃人物，是gold连接器和GCC过程间优化LTO的主要设计者之一，是Zembu公司的创始人之一
- Russ Cox
- Brad Fitzpatrick

### Hello, world

```
package main

import "fmt"

func main() {
  fmt.Printf("Hello, 世界\n")
}
```

### Hello, world 2.0

Serving “Hello, world” at [http://localhost:8080/world](http://localhost:8080/world)

```
package main

import (
  "fmt"
  "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
  fmt.Fprint(w, "Hello, "+r.URL.Path[1:])
}
func main() {
  http.HandleFunc("/", handler)
  http.ListenAndServe(":8080", nil)
}
```

## 类型
---------------

### 变量

- 使用关键字 var
  var x int
  var f float32 = 1.6
  var s = "abc"

- 在函数内部，可以用 ":="
  func main() {
    // 只能在函数里这样赋值
    x := 123 // 注意检查,是定义新局部变量,还是修改全局变量。该方式容易造成错误。
  }

- 可以一次定义多个变量
  var x, y, z int
  var s, n = "abc", 123

  var (
    a int
    b float32
  )

### 常量

常量值必须是编译期可确定的数字、字符串、布尔值

```
package main

const x, y int = 1, 2     // 多常量初始化
const s = "Hello, World!" // 类型推断

const ( // 常量组
  a, b      = 10, 100
  c    bool = false
)

func main() {
  const x = "xxx"   // 未使用用局部常量不会引发编译错误。
  //! var y = "yyy" // 编译错误
  println("ok")
}

```

### 基本类型

- bool
- int
- float
- complex
- array
- struct
- string
- slice
- map
- channel
- interface
- function

### 自定义类型

```
package main

import (
  "fmt"
)

type MyFloat float64

func (m MyFloat) Abs() float64 {
  f := float64(m)
  if f < 0 {
    return -f
  }
  return f
}

func main() {
  f := MyFloat(-42)
  ff := float64(f)

  fmt.Printf("%f, ===ff=%v, %v\n", f.Abs(), ff, f == ff)      //=> 42.0, ff=-42
  //! fmt.Printf("%f, ===ff=%v, %v\n", f.Abs(), ff, f == ff)  // 编译错误，f 和 ff 相比较时 go 会进行强制类型检查
}
```

## Go 的好
------------

- 快！
  开发效率和运行效率
- 编译快速简单，最后就一个可执行文件
  一个平台下可编译出其他平台的执行文件
- 强大标准库
- 天生的并发支持
- interface
- GC
- ...

## Go 适合用来做什么

- 服务器编程，以前你如果使用 C/C++ 做的那些事情，用Go来做很合适，例如处理日志、数据打包、虚拟机处理、文件系统等
- 分布式系统，数据库代理器等
- 网络编程，这一块目前应用最广，包括Web应用、API应用、下载应用
- 内存数据库，前一段时间 google 开发的 groupcache，couchbase 的部分组建
- 云平台，目前国外很多云平台在采用 Go 开发，CloudFoundy 的部分组建，前VMare的技术总监自己出来搞的 apcera 云平台
- Api

## 想做网站？

- beego: https://github.com/astaxie/beego
- martini: https://github.com/go-martini/martini
- revel: https://github.com/revel/revel

## 知名的 Go 项目

- Docker: 基于lxc的一个虚拟打包工具，能够实现PAAS平台的组建
- CoreOS: 打造一个随时可被替换的操作系统，甚至在这个替换的过程中，应用程序的运行不会被打断
- vitess: Youtube 出品的开源分布式 MySQL 工具集 Vitess
- NSQ: bitly 开源的高性能消息队列系统，目前他们每天处理数十亿条的消息
- Heka: mazila 开源的日志处理系统
- Kubernetes: 来自 Google 云平台的开源容器集群管理系统
- groupcache: memcache 作者写的用于 Google 下载系统的缓存系统

## 国外使用 Go 的公司

- Google
- Facebook
- Heroku
- GitHub
- Dropbox
- 10gen

[https://code.google.com/p/go-wiki/wiki/GoUsers](https://code.google.com/p/go-wiki/wiki/GoUsers)

## 国内使用 Go 的公司

- 七牛
- 百度
- 360
- 盛大
- 美团
- 陌陌
- 知乎
- 豆瓣
- ...

## 进展

- 5 周岁，刚发布 1.4
- 支持平台
  FreeBSD 8 or later
  Linux 2.6.23 or later with glibc
  Mac OS X 10.6 or late
  Windows XP or later
- 1.4 支持 Android（NDK）
- 1.5 要自举、支持 iOS、并行 GC ...

## Q & A
----------------

**参考资料**

[http://www.golang.org](http://www.golang.org)

[http://www.golanghome.com](http://www.golanghome.com)

[https://github.com/mikespook/Learning-Go-zh-cn](https://github.com/mikespook/Learning-Go-zh-cn)

[https://github.com/astaxie/build-web-application-with-golang](https://github.com/astaxie/build-web-application-with-golang)

[https://github.com/qyuhen/book](https://github.com/qyuhen/book)

## 广告
--------------

**广州舜飞信息科技**

- 招人
- 实习生
