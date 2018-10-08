```go
[GD-0103-0121@rjsys ~/go/src/client2]$ cat c2.go
package main

import (
        "fmt"
        "io"
        "net"
        "net/http"
        "os"
        "time"
)

func main() {
        c := http.Client{
                Transport: &http.Transport{
                        Dial: func(netw, addr string) (net.Conn, error) {
                                deadline := time.Now().Add(5 * time.Second)
                                c, err := net.DialTimeout(netw, addr, time.Second*5)
                                if err != nil {
                                        fmt.Println("error!!!!!!!!!!!!!!!!!!!!")
                                        return nil, err
                                }
                                c.SetDeadline(deadline)
                                return c, nil
                        },
                },
        }

        //reqest, err := c.Get("http://google.com")
        // url := "http://google.com"
        url := "http://is.snssdk.com/api/news/feed/v51/"
        reqest, _ := http.NewRequest("GET", url, nil)
        response, err := c.Do(reqest)
        if err != nil {
                println(err)
                return
                // panic(err)
        }
        stdout := os.Stdout
        _, err = io.Copy(stdout, response.Body)

        status := response.StatusCode
        fmt.Println(status)

}
[GD-0103-0121@rjsys ~/go/src/client2]$

```
