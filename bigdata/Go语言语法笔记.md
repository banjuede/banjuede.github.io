# 变量
---
## 1. 变量赋值可以有三种方式：
> `var value value_type`,  
> `value = 10`,  
> `value := 10`,   
> 其中这种因式分解关键字的写法一般用于声明全局变量  
> ```
> var (
> 	a int
> 	b bool
> )
> ```  
> 而 `value := 10` 这种不带声明格式的只能在函数体中出现
	
## 2. 多变量并行(同时)复制
> 1. `a, b, c = 5, 7, "abc"`, 所以交换两个变量的值就可以简单的这样做：`a, b := b, a`, 还比如当一个函数返回多个返回值时，比如这里的 val 和错误 err 是通过调用 Func1 函数同时得到：`val, err = Func1(var1)`   
> 2. 空白标识符` _` 也被用于抛弃值，如值 5 在：_, b = 5, 7 中被抛弃, `_` 实际上是一个只写变量，你不能得到它的值,这样做是因为 Go 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值  
> 3. 需要注意的一点，如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误  
	
## 3. 变量作用域
> Go 语言程序中全局变量与局部变量名称可以相同，但是函数内的局部变量会被优先考虑。实例如下：
> ```go
> package main
> 
> import "fmt"
> 
> /* 声明全局变量 */
> var g int = 20
> 
> func main() {
>    /* 声明局部变量 */
>    var g int = 10
> 
>    fmt.Printf ("结果： g = %d\n",  g)
> }
> ```
	
# 常量
---
> 1. 常量可以使用因式分解关键字的写法当作枚举  
> ```
> const (
> 	Unknown = 0
> 	Female = 1
> 	Male = 2
> )
> ```  
> 2. 同时常量也可以用len(), cap(), unsafe.Sizeof()常量计算表达式的值，常量表达式中，函数必须是内置函数，否则编译不过  
> 3. 还有一个特殊常量 `ota`，可以认为它是一个可以被编译器修改的常量，在每一个const关键字出现时`ota`被重置为0，在下一个`const`出现之前，每出现一次`iota`其所代表的数字会自动增加1
	
# 函数
---
## 1. 函数可以有多个返回值
> 函数参数传递默认是值传递
> ```
> func swap(x, y string) (string, string) {
> 	return y, x
> }
> ```  
## 2. 函数作为值。
> 以下实例中我们在定义的函数中初始化一个变量，该函数仅仅是为了使用内置函数 math.sqrt() ，实例为：  
> ```
> package main
> 
> import (
>    "fmt"
>    "math"
> )
> 
> func main(){
>    /* 声明函数变量 */
>    getSquareRoot := func(x float64) float64 {
>       return math.Sqrt(x)
>    }
> 
>    /* 使用函数 */
>    fmt.Println(getSquareRoot(9))
> 
> }
> ```  
## 3. 闭包的支持
> Go 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。  
> 以下实例中，我们创建了函数 getSequence() ，返回另外一个函数。该函数的目的是在闭包中递增 i 变量，代码如下：
> ```go
> package main
> 
> import "fmt"
> 
> func getSequence() func() int {
>    i := 0
>    return func() int {
>       i += 1
>       return i  
>    }
> }
> 
> func main() {
>    /* nextNumber 为一个函数，函数 i 为 0 */
>    nextNumber := getSequence()  
> 
>    /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
>    fmt.Println(nextNumber()) //1
>    fmt.Println(nextNumber()) //2
>    fmt.Println(nextNumber()) //3
>    
>    /* 创建新的函数 nextNumber1，并查看结果 */
>    nextNumber1 := getSequence()  
>    fmt.Println(nextNumber1()) //1
>    fmt.Println(nextNumber1()) //2
> }
> ```  
## 4. 方法的支持
> Go 语言中同时有函数和方法。一个方法就是一个包含了接受者的函数，接受者可以是命名类型或者结构体类型的一个值或者是一个指针。所有给定类型的方法属于该类型的方法集。语法格式如下：
> ```
> func (variable_name variable_data_type) function_name() [return_type]{
>    /* 函数体*/
> }
> ```  
> 下面定义一个结构体类型和该类型的一个方法：
> ```
> package main
> 
> import (
>    "fmt"  
> )
> 
> /* 定义函数 */
> type Circle struct {
>   radius float64
> }
> 
> func main() {
>   var c1 Circle
>   c1.radius = 10.00
>   fmt.Println("Area of Circle(c1) = ", c1.getArea())
> }
> 
> //该 method 属于 Circle 类型对象中的方法
> func (c Circle) getArea() float64 {
>   //c.radius 即为 Circle 类型对象中的属性
>   return 3.14 * c.radius * c.radius
> }
> ```
> 以上代码执行结果为：
> `Area of Circle(c1) =  314`

# 指针
---
> go语言中的指针和c语言中的指针类似就不做过多解释，声明格式如下：  
> `var var_name *var-type`  
> var-type 为指针类型，var_name 为指针变量名，* 号用于指定变量是作为一个指针。以下是有效的指针声明：
> `var ip *int        /* 指向整型*/`  
> `var fp *float32    /* 指向浮点型 */`  
> 本例中这是一个指向 int 和 float32 的指针。  
> 不同的是go语言的空指针为`nil`而不是null，类似于c语言还有`指针数组,指针的指针，传递指针参数`等

# 结构体
---
> 结构体也类似于C语言的结构体也不做过多解释
> 结构体定义需要使用 type 和 struct 语句。struct 语句定义一个新的数据类型，结构体有中一个或多个成员。type 语句设定了结构体的名称。结构体的格式如下：
> ```
> type struct_variable_type struct {
>    member definition;
>    member definition;
>    ...
>    member definition;
> }
> ```  
> 一旦定义了结构体类型，它就能用于变量的声明，语法格式如下：  
> `variable_name := structure_variable_type {value1, value2...valuen}`  
>  如果要访问结构体成员，需要使用点号 `.` 操作符，格式为：`结构体.成员名`

# 切片
---
> go语言中的切片就相当于java中的List，和数组相对它的长度可变，其声明格式如下：
> `var identifier []type`  
> 切片不需要说明长度，或使用make()函数来创建切片：  
> `var slice1 []type = make([]type, len)`  
> 也可以简写为：  
>  `slice1 := make([]type, len)`  
>  也可以指定容量，其中capacity为可选参数：  
>  `make([]T, len, capacity)`  
>  其中`len`表示切片长度，`capacity`表示切片容量(即最大长度)