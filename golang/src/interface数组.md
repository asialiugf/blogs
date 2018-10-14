# 注意他们的区别
### []interface{} 这里是 interface数组
既然空的 interface 可以接受任何类型的参数，那么一个 interface{}类型的 slice 是不是就可以接受任何类型的 slice ?
```go
func printAll(vals []interface{}) { //1
	for _, val := range vals {
		fmt.Println(val)
	}
}
func main(){
	names := []string{"stanley", "david", "oscar"}
	printAll(names)
}
```
上面的代码是按照我们的假设修改的，执行之后竟然会报 cannot use names (type []string) as type []interface {} in argument to printAll 错误，why？

这个错误说明 go 没有帮助我们自动把 slice 转换成 interface{} 类型的 slice，所以出错了。go 不会对 类型是interface{} 的 slice 进行转换 。为什么 go 不帮我们自动转换，一开始我也很好奇，最后终于在 go 的 wiki 中找到了答案 https://github.com/golang/go/wiki/InterfaceSlice 大意是 interface{} 会占用两个字长的存储空间，一个是自身的 methods 数据，一个是指向其存储值的指针，也就是 interface 变量存储的值，因而 slice []interface{} 其长度是固定的N*2，但是 []T 的长度是N*sizeof(T)，两种 slice 实际存储值的大小是有区别的(文中只介绍两种 slice 的不同，至于为什么不能转换猜测可能是 runtime 转换代价比较大)。

但是我们可以手动进行转换来达到我们的目的。
```go
var dataSlice []int = foo()
var interfaceSlice []interface{} = make([]interface{}, len(dataSlice))
for i, d := range dataSlice {
	interfaceSlice[i] = d
}
```


### sort源码，type IntSlice []int 这样是可以的
* https://github.com/golang/go/blob/master/src/sort/sort.go

```go
type Interface interface {
	// Len is the number of elements in the collection.
	Len() int
	// Less reports whether the element with
	// index i should sort before the element with index j.
	Less(i, j int) bool
	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}
```

```go
func Sort(data Interface) {
	n := data.Len()
	quickSort(data, 0, n, maxDepth(n))
}

```

```go
// IntSlice attaches the methods of Interface to []int, sorting in increasing order.
type IntSlice []int

func (p IntSlice) Len() int           { return len(p) }
func (p IntSlice) Less(i, j int) bool { return p[i] < p[j] }
func (p IntSlice) Swap(i, j int)      { p[i], p[j] = p[j], p[i] }

// Sort is a convenience method.
func (p IntSlice) Sort() { Sort(p) }
```
