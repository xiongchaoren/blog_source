---
title: nodejs-fs模块
date: 2019-06-17 13:12:55
tags: fs
categories: nodes
---

## 文件操作

### 创建/写入文件

```javascript
var fs = require('fs');
fs.writeFile('a.txt', '', {			//写入数据类型: string,buffer,url,fd(即文件描述符)
  encoding: 'utf8',
  mode: '0o666',			// 权限，可通过chmod更改
  flag: 'w'    //a追加,w写入,r读取，a+读取和追加,w+读取和写入,r+读取和写入,'a'、'a+'、'w'、'w+'下如果文件不存在则创建文件
}, (err) => {
  if (err) throw err;
  console.log('文件创建/写入成功');
});
```

### 读取文件(参数与writeFile相同)

```javascript
var fs = require('fs');
fs.readFile('a.txt', (err, data) => {
  if (err) throw err;
  console.log('读取文件内容: ', data);
});
```

### 文件描述符和文件写入读取关闭操作

```javascript
var fs = require('fs');
fs.open('a.txt', 'a+', (err, fd) => {
  if (err) throw err;
  var write_data = '写入数据';
  // 或者write_data可以是buffer
  fs.write(fd, '写入数据', write_data, 0, 'utf8', (err, written, data) => {
    if (err) throw err;
    console.log('写入数据的字节数: ', written);
    console.log('写入的数据: ', data);     // 如果传入的是buffer，则返回的是buffer
    
    fs.read(fd, Buffer.alloc(8), 0, 8, null, (err, bytesRead, buffer) => {
      if (err) throw err;
      console.log('读取字节数: ', bytesRead);
      console.log('写入buffer', buffer.toString());
      
      fs.close(fd, (err) => {
        if (err) throw err;
        console.log('关闭成功');
      });
    })
  })
})
```

### 删除文件/软链接

```javascript
var fs = require('fs');
fs.unlink('a.txt', (err) => {
  if (err) throw err;
  console.log('文件已删除');
});
```

### 复制文件

```javascript
var fs = require('fs');
fs.copyFile('a.txt', 'dest/a.txt', (err) => {
  if (err) throw err;
  console.log('源文件已拷贝到目标文件');
});
```

### 创建文件夹

```javascript
var fs = require('fs');
fs.mkdir('dest/a',{ recursive: true }, (err) => {
  if (err) throw err;
  console.log('文件夹创建成功');
});
```

### 删除文件夹

```javascript
var fs = require('fs');
fs.rmdir('dest/a', (err) => {
  if (err) throw err;
  console.log('文件夹删除成功');
});
```

### 重命名(或者移动)文件/文件夹

```javascript
var fs = require('fs');
fs.rename('dest/a', 'dest/b' (err) => {
  if (err) throw err;
  console.log('文件夹重命名成功');
});
```

### 读取文件夹

```javascript
var fs = require('fs');
fs.readdir('dest/a', (err, files) => {   //files是数组，元素为文件/文件夹名
  if (err) throw err;
  console.log('文件夹下资源: ', files);
});
```

### 读取文件/文件夹状态

```javascript
var fs = require('fs');
fs.stat('dest/a.txt', (err, stats) => {   //stats是类fs.Stat的实例
  if (err) throw err;
  console.log('文件状态: ', stats);
});
```