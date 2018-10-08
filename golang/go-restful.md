### 下载
```
  502  cd $GOPATH
  504* go get github.com/emicklei/go-restful\
  505  go install go-restful
  506  go install emicklei/go-restful
  507  go install github.com/emicklei/go-restful
```
### 编译运行 restful-curly-router.go
```go
[GD-0103-0121@rjsys ~/go/src/github.com/emicklei/go-restful/examples]$ ll
[GD-0103-0121@rjsys ~/go/src/restful]$ ll
total 7260
-rwxr-xr-x 1 GD-0103-0121 197121 7427584 十月  8 12:53 restful.exe*
-rw-r--r-- 1 GD-0103-0121 197121    3057 十月  8 12:53 restful-curly-router.go
[GD-0103-0121@rjsys ~/go/src/restful]$

$ go build
$ ./restful.exe&
```

### POST 数据
```
[GD-0103-0121@rjsys ~/go]$ curl -H "Content-Type:application/json" -X POST -d '{"Id": "43a", "Name":"12345678"}' http://127.0.0.1:8080/users
[GD-0103-0121@rjsys ~/go]$ curl -H "Content-Type:application/json" -X POST -d '{"Id": "admin", "Name":"12345678"}' http://127.0.0.1:8080/users
[GD-0103-0121@rjsys ~/go]$ curl -H "Content-Type:application/json" -X POST -d '{"Id": "admin", "Name":"tt12345678"}' http://127.0.0.1:8080/users
```
### 浏览器打开 http://127.0.0.1:8080/users/admin  http://127.0.0.1:8080/users/43a
显示
```
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<User>
<Id>admin</Id>
<Name>tt12345678</Name>
</User>
```
