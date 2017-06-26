# Stream

在node中，流是由几个不同对象附着的抽象接口。主要有可读流与可写流。

node中流的使用场景：

* net
* fs
* ......

## 可读流

### 等待数据

流通过监听`data`事件来等待数据的接收。

```js
stream.on('data', data => {
    // code
})
```

### 暂停/恢复流

可读流就像一个阀门，可以通过`关闭阀门`来暂停流

```js
// 暂停流，监听的`data`的事件将不会接收到数据
stream.pause()
```

> 暂停在不同的情况会有不一样的行为。文件流会直接停止从该文件中读取数据；TCP套接字讲不再会读取新的数据包，也会终止其他终端过来的数据包流

```js
// 恢复流
stream.resume()
```

### 终止

流在结束时发送`end`事件。可以通过监听这个事件来知道合适结束

```js
stream.on('end', () => {
    // code
})
```

## 可写流

可写流顾名思义就是可以向流中写入数据

```js
const isWriteInBuffer = stream.write('string', 'utf8')
```

向流中写入数据，一般会将数据传递到缓冲区中，此时`isWriteInBuffer=true`;如果没有，会在写入一个队列保存在内存中,此时`isWriteInBuffer=false`。

## drain事件

当流刷新挂起的缓冲区时，会发射`drain`事件

```js
stream.on('drain', () => {
    // code
})
```

## 内置fs

fs在读取和操作大文件时，如果没有流需要等待fs读取完整个文件塞入内存之后对其操作。这个过程是阻塞的，当遇到很大文件时影响效率且可能因为文件过大而出现溢出情况。
使用流接口读取，极大的解决了这个问题

```js
const filenameReadStream = fs.createReadStream('./filename.txt')
filenameReadStream.on('data', data => {
    // read
})
filenameReadStream.on('end', () => {
    // end
})
filenameReadStream.on('error', err => {
    // err
})
```

### 利用pipe管道

```js
const http = require('http')
const fs = require('fs')
const zlib = require('zlib')
http.createServer((req, res) => {
    res.writeHead(200, {
        'Content-Encoding': 'gzip'
    })
    const filenameReadStream = fs.createReadStream('./filename.txt')
    filenameReadStream.on('error', err => {
        // err
    })
    filenameReadStream.pipe(zlib.createGzip())
                      .pipe(res)
}).listen(3000)
```

### 流的继承

#### 基础流

* Readable: 创建一个可读流接口
* Writeable: 创建一个可写流接口
* Duplex/Transfrom: 创建可以修改流数据格式的双工流接口
* PassThrough: 用于测试分析，但是不修改流信息的接口

```js
const Readable = require('stream').Readable
class SomeStream extends Readbale {
    constructor(options) {
        super(options)

    }

    _read() {
        // 重新 _read 方法
    }
}

```

### 实例

* 解析.txt
* 对.txt进行分段。
* 创建服务访问

```js
const http = require('http')
const fs = require('fs')
const stream = require('stream')

class ToStream extends stream.Duplex {
    constructor(options) {
        super(options)
        this.isWriting = false
    }
    _read(size) {
        if(!this.isWriting) {
            this.isWriting = true
        }
    }

    _write(chunk, encoding, cb) {
        this.isWriting = false
        let chunkStr = chunk.toString()
        let chunkStrArr = chunkStr.split(/,|[\r\n]/g)
        let rArr = chunkStrArr.map(item => {
            return `<p>${item}</p>`
        })
        this.push(rArr.join(''))
        cb()
    }

}

http.createServer((req, res) => {
    res.writeHead(200, {
        "Content-Type": "text/html"
    })
    const read = fs.createReadStream('./aa.txt')
    read.pipe(new ToStream())
        .pipe(res, {
            end: false
        })
    read.on('end', () => {
        res.end()
    })
}).listen(3000)
```
