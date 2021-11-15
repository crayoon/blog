# NodeJS 相关

> 自动绑定端口

- 直接监听0端口
```
server.listen(0); 
server.on('listening', function() { var port = server.address().port });
```
- 使用扩展
```
https://github.com/sindresorhus/get-port
```

