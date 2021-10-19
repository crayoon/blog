## 启动
```
mongod -f /etc/mongod.conf --fork
```
## 重启
```
# 进入shell
/usr/bin/mongo
# 使用admin
use admin
# 停止指令
db.shutdownServer({timeoutSecs: 60});
# 再执行启动
mongod -f /etc/mongod.conf --fork
```

注意：
非正常关闭MongoDB是会产生lock文件和repair文件，
测试一般的kill + 进程号操作后，删除lock文件，使用 --repair进行恢复即可；
kill -9 进程号 无法进行恢复，此时需要丢失部分操作数据才可启动，为不完整修复吧