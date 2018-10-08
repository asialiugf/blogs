###
```go
[GD-0103-0121@rjsys ~/go/src/base64]$ cat a.go
package main

import (
        "bytes"
        "compress/zlib"
        "encoding/base64"
        "fmt"
        "io"
        "io/ioutil"
        "log"
        "os"
)

const base64Table = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
const base64Table1 = "1ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789a"

var coder = base64.NewEncoding(base64Table)
var coder1 = base64.NewEncoding(base64Table1)

func Base64Encode(encode_byte []byte) []byte {
        return []byte(coder.EncodeToString(encode_byte))
}
func Base64Decode(decode_byte []byte) ([]byte, error) {
        return coder.DecodeString(string(decode_byte))
}

func Base64Encode1(encode_byte []byte) []byte {
        return []byte(coder1.EncodeToString(encode_byte))
}
func Base64Decode1(decode_byte []byte) ([]byte, error) {
        return coder1.DecodeString(string(decode_byte))
}
func main() {
        file, err := os.Open("./test.jpg")
        if err != nil {
                log.Fatal(err)
        }
        data, err := ioutil.ReadAll(file)
        if err != nil {
                log.Fatal(err)
        }
        //  -----------------------------------------------------
        Strr := Base64Encode(data) // encode
        // fmt.Printf("Data as string: %s\n", Base64Encode(data))
        fmt.Printf("Data as string: %s\n", Strr)
        fmt.Println("=========================")

        Strr1 := Base64Encode1(data) // encode
        fmt.Printf("Data as string: %s\n", Strr1)
        fmt.Println("=========================")

        var data1 []byte
        data1, _ = Base64Decode(Strr) //decode
        if ioutil.WriteFile("./test1.jpg", data1, 0644) == nil {
                fmt.Println("写入文件成功:")
        }
        //  -----------------------------------------------------
        file_json, err := os.Open("./jcode.txt")
        if err != nil {
                log.Fatal(err)
        }
        data_json, err := ioutil.ReadAll(file_json) // read the json file to data_json
        if err != nil {
                log.Fatal(err)
        }

        var in bytes.Buffer
        // b := []byte(`xiorui.cc`)
        w := zlib.NewWriter(&in) // compress!!
        w.Write(data_json)
        w.Close()

        if ioutil.WriteFile("./jcode.zip", in.Bytes(), 0644) == nil { // write to file jcode.zip
                fmt.Println("写入zip文件成功:")
        }

        var oot bytes.Buffer
        rr, _ := zlib.NewReader(&in) // uncompress !!
        io.Copy(&oot, rr)
        fmt.Println(oot.String())
        fmt.Println("--------------------------")

        //  -----------------------------------------------------
        file_zip, err := os.Open("./jcode.zip")
        if err != nil {
                log.Fatal(err)
        }
        data_zip, err := ioutil.ReadAll(file_zip)
        if err != nil {
                log.Fatal(err)
        }

        var out bytes.Buffer
        bb := bytes.NewReader(data_zip)
        r, _ := zlib.NewReader(bb)
        io.Copy(&out, r)
        fmt.Println(out.String())
        fmt.Println("--------------------------")
        return

}
[GD-0103-0121@rjsys ~/go/src/base64]$

```
