### 下载 git clone https://github.com/golang/tools.git tools
### 移到相应的目录下 mv tools $GOPATH/src/golang.org/x/
### 下载 go get -u github.com/stamblerre/gocode
### 安装 go install golang.org/x/lint/golint

```
Installing github.com/mdempsky/gocode SUCCEEDED
Installing github.com/uudashr/gopkgs/cmd/gopkgs SUCCEEDED
Installing github.com/derekparker/delve/cmd/dlv SUCCEEDED
Installing github.com/rogpeppe/godef SUCCEEDED
Installing golang.org/x/tools/cmd/guru FAILED
Installing golang.org/x/tools/cmd/gorename FAILED

Installing github.com/ramya-rao-a/go-outline FAILED
Installing github.com/acroca/go-symbols FAILED
Installing github.com/stamblerre/gocode FAILED
Installing github.com/ianthehat/godef FAILED
Installing github.com/sqs/goreturns FAILED
Installing github.com/golang/lint/golint FAILED

git clone https://github.com/golang/tools.git tools
mv tools $GOPATH/src/golang.org/x/

git clone  https://github.com/golang/lint
mv ./lint ~/go/src/golang.org/x/lint
go install golang.org/x/lint/golint
```

### $GOPATH/src 目录
```
go install golang.org/x/tools/cmd/guru
go install golang.org/x/tools/cmd/gorename
go install golang.org/x/lint/golint

go get -u github.com/stamblerre/gocode
go get -u github.com/derekparker/delve/cmd/dlv
go get -u github.com/josharian/impl
go get -u github.com/rogpeppe/godef
go get -u github.com/sqs/goreturns
go get -u github.com/golang/lint/golint
go get -u github.com/cweill/gotests/gotests
go get -u github.com/ramya-rao-a/go-outline
go get -u github.com/acroca/go-symbols
```

