```golang
[GD-0103-0121@rjsys ~/go/src/base64]$ cat a.go
package main

import (
        "encoding/base64"
        "fmt"
        "io/ioutil"
        "log"
        "os"
)

const base64Table = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"

var coder = base64.NewEncoding(base64Table)

func main() {
        file, err := os.Open("./test.jpg")
        if err != nil {
                log.Fatal(err)
        }
        data, err := ioutil.ReadAll(file)
        if err != nil {
                log.Fatal(err)
        }
        Strr := Base64Encode(data)
        // fmt.Printf("Data as string: %s\n", Base64Encode(data))
        fmt.Printf("Data as string: %s\n", Strr)
        var data1 []byte
        data1, _ = Base64Decode(Strr)
        if ioutil.WriteFile("./test1.jpg", data1, 0644) == nil {
                fmt.Println("写入文件成功:")
        }
}
func Base64Encode(encode_byte []byte) []byte {
        return []byte(coder.EncodeToString(encode_byte))
}
func Base64Decode(decode_byte []byte) ([]byte, error) {
        return coder.DecodeString(string(decode_byte))
}
[GD-0103-0121@rjsys ~/go/src/base64]$

```
