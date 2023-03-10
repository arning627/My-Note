# 基础概念:
* main包下 必须创建main函数为程序入口
* 调用同包函数不需要文件

# 数据类型:
### 基本数据类型:
* 整型：

 | 类型   | 占用存储   | 表数范围 | 备注     |
| :---   |    :----:   |     :---:   |:----:|
| int       | 32位系统4字节 64位系统8字节   | Here's this   |-|
| int8     | 1字节       | And more      |-|
| int16    | 2字节       | And more      |-|
| int32    | 4字节      | And more      |-|
| int64    | 8字节      | And more      |-|
| uint      | 32位系统4字节 64位系统8字节  | And more  |-|
| uint8    | 1字节       | And more      |-|
| uint16   | 2字节      | And more      |-|
| uint32  | 4字节      | And more      |-|
| uint64  | 8字节      | And more      |-|
| rune    |  4字节      |				|等价于int32 表示Unicode码 |
| byte    | 1字节        |  0～255	|- |

-

* 浮点

  | 类型   | 占用存储   | 表数范围 | 备注     |
  | :----| :-----:| :----:| :----:|
  | float32| 4字节 | - | 单精度|
  | float64| 8字节 | - | 双精度| 
    	  
* 字符类 没有专有类型，使用byte 来保存单个字母字符
* 布尔 bool
    - true
    - false
* 字符串
    - string（go官方将string归属为基本数据类型)一个字符占3个字节，字符串按下标方式获取到的为每个字节的十进制形式

-
 
基本数据类型的转换: 
	
```go
//表达式 T(v) ，将v转换为类型T
var i int32 = 100
//将 int32类型 转换为 float32类型
var i float32 = float32(i)
```
-
    
### 派生/复杂数据类型:
* 指针 Pointer

	+ 典型的引用拷贝

	```go
	//*[Type] 声明指针变量 type代表数据指向的数据类型
	//&[name] 得到当前变量的指针
	//*[name] 得到指针指向的值
	var ptr *int //声明一个int类型的指针
	*ptr = 10 // 赋值	
	```
	
* 数组 
	
	+ 数组的第一个元素地址就是数组的地址
	+ 数组内元素地址间隔是由元素类型所占字节大小决定
	+ 数组默认进行值传递，传递过程会进行值拷贝，需要在其他函数中变更数组元素值需要传递指针（引用传递）

	```go
	//声明一个长度为20的int数组
	var int_arrays [20]int
	int_arrays[0] = 1
	int_arrays[1] = 3
	
	```

* 结构体 struct
	
	+ 相当于Java中的对象，可定义属性，绑定函数
	+ 属于值类型 传递进行值拷贝


	```go
	type Cat struct {
	    Name  string
	    Age   int
	    Color string
	}
	func main() {
	    var cat Cat
	    cat.Name = "cat name"
	    cat.Age = 10
	    cat.Color = "red"
	}
	```
 	+ 绑定方法:

	```golang\
	type Cat struct {
	    	Name  string `json:"name"`
	    	Age   int    `json:"age"`
	    	Color string `json:"color"`
	}
	func (cat Cat) eat() {
		//值传递 不会改变原对象属性
		cat.Name = "robot"
	}
	func (c *Cat) eat_1() {
		// 引用传递 会改变原对象值
		c.Name = "robot"
	}
	```


* 管道 Channel
* 函数 func
	
	+ 在Go中也算作一种数据类型
	
	``` go
	func name(){
		//.....具体逻辑
	}
	```
	
* 切片 slice
	+ 可以理解为是一个可变长度的数组，初始容量为3，每次自增容量长度为之前的2倍，在容量超过1024之后，每次增加之前长度的1/4
	+ 切片 是数组的引用 所以为引用类型 在传递时遵循引用传递机制
	+ 相当于Java中的List\<T>
    
    
 
  ```
	//创建切片
	var num []int 
	var  data = []int{1,3,5}
	data := []int{1,3,5}
	var datas = make([]table)//1代表 长度，
	```
   
* 接口 interface
	
	+ 在Go中体现多态
	+ 接口的数据类型是指针（引用类型）
	+ interface中不能包含变量 ，不需要显示实现
	+ 有个对象包含接口中的所有方法就算实现 
	+ 两个接口都拥有相同的方法时，包含所有方法的对象相当于同时实现两个接口
	+ 自定义类型都可以实现接口
	+ 接口继承 语法与结构体继承一致 接口代码块中声明匿名接口类型
	+ 实现接口时 如果该继承了其他接口 同样需要实现父接口中的所有方法才可
	+ 空接口 interface{} 没有任何方法 相当于所有类型都实现了该接口

	
	```Go
	type AInterface interface {
		say()	
	}
	type BInterface interface {
		hello()
	}
	type Monster struct {
	}
	func (m Monster) say() {
		fmt.Println("say()")
	}
	func (m Monster) hello() {
		fmt.Println("hello()")
	}
	func main() {
		var aInterface AInterface = Monster{}
		aInterface.say()
		var bInterface BInterface = Monster{}
		bInterface.hello()
	}
	```
	
* map

	+ key-value数据结构
	+ 声明时不会分配内存 需要用make开辟内存空间
	+ 无序存储
	+ var 变量名 map[keyType]valueType
	+ 不能以slice,map,func 作为key的类型
	
	创建和赋值：
	
	```Go
	func main() {
		//1
		var kv map[string]string
		kv = make(map[string]string, 10)
		kv["A"] = "a"
		kv["B"] = "b"
		kv["C"] = "c"
		//2
		kv_1 := make(map[string]string, 5)
		kv_1["a"] = "A"
		//3 
		kv_2 := map[string]string{
			"A": "a",
			"B": "b",
		}
	}
	```
	
	调用：
	

# 保留关键字:
- break
- default
- func
- interface
- select
- case
- defer
- go
- map
- struct
- chan
- else
- goto
- package
- switch
- const
- fallthrough
- if
- range
- type
- continue
- for
- import
- return
- var
- new 

iota 常量定义计数器，从0开始，用以生成连续常量，只作用于因式分解区域


new关键字返回的为指针类型
make 返回为对象

浮点类型转换为二进制时：
	整数部分直接转换，小数部分转换规则为：
		小数部分*2，如果结果小于1，则继续*2，如果结果大于1，则结果-1后的小数部分继续*2，直到结果刚好等于1

布尔类型：
	字符串转布尔类型时 "1","t","T","true","True" 转换后都为true 	"0","f","F","false","False" 转换后都为false

字符类型
	一个字符3个字节，字符串按下标方式获取到的为每个字节的十进制形式
切片类型
	可以理解为是一个可变长度的数组，初始容量为3，每次自增容量长度为之前的2倍，在容量超过1024之后，每次增加之前长度的1/4
	切片结构：
		属性有 array 指针，len 长度，cap 容量
	创建切片：
	var num []int 
	var  data = []int{1,3,5}
	data := []int{1,3,5}
	var datas = make([]int,1,3)  // 1 代表 长度，4 代表初始化容量， 实际含义为 创建一个切片，切片内数据初始化数据为1（第一个元素有值，int类型默认为0） 容量为3，其中第二第三元素为nil

datas := append(data,3) 含义为在data切片中添加一个新的元素，值为3， 如果append()触发了自动扩容，会创建一块新的内存地址来存放切片数据并返回。


简略声明常量，如需从1开始，写法为 v1 = iota +1

	*[Type] 声明指针变量 type代表数据指向的数据类型
	&[name] 得到当前变量的指针
	*[name] 得到指针指向的值

### 闭包：
	函数使用函数外的参数完成调用 参数与函数称为闭包
	

defer  
	defer修饰的语句会被压入独立的栈区 暂时不执行 在执行完代码块后再以弹栈执行（代码块以函数为边界 压入时做值拷贝）
	
参数传递
	1 值传递 （基本数据类型 数组 结构体 string）
	2 引用传递 （指针 slice切片 map 管道chan interface等）
	
	值传递是值的拷贝
	引用传递是地址拷贝
	
异常处理：
	使用revover()函数接受 err
	使用panic()抛出异常
	
	
	func errorOne() {
	    defer func() {
	        err := recover()
	        if err != nil {
	            fmt.Print(err) //正常输出 不阻断
	            panic(err)     //抛出异常 阻断进程
	        }
	    }()
	    num := 0
	    res := 10 / num
	    fmt.Print(res)
	}
结构体:
	字段/属性
	行为/方法
	与Java中的class是相同地位
	Go中 结构体是值类型
	
	
	type Cat struct {
	    Name  string
	    Age   int
	    Color string
	}
	func main() {
	    var cat Cat
	    cat.Name = "cat name"
	    cat.Age = 10
	    cat.Color = "red"
	}
	
	声明结构体:
		
	    var cat Cat
	
	    cat1 := Cat{}
	
	    cat2 := Cat{"c", 2, "black"}
	
		//结构体指针
	    var cat3 *Cat = new(Cat)
	    //标准写法
	    (*cat3).Name = "d"
	    //可简写
	    cat3.Age = 12
	
	    //简写方式同上
	    var cat4 *Cat = &Cat{}
	    cat4.Name = "e"
	
		var cat5 *Cat = &Cat{"f", 20, "blue"}
	
	结构体通常会为字段添加一个tag 表明进行json序列化时 key的名称
		
		type Cat struct {
		    Name  string `json:"name"`
		    Age   int    `json:"age"`
		    Color string `json:"color"`
		}
	添加tag后 序列化后json串中key为tag中指定 通过反射实现
	
	
	结构体方法定义：
		
		type Cat struct {
		    Name  string `json:"name"`
		    Age   int    `json:"age"`
		    Color string `json:"color"`
		}
		func (cat Cat) eat() {
			// 在调用时 cat表明具体对象实例
		    fmt.Print(cat.Name)
		}
		
		表明Cat对象包含一个方法 eat()
		
		
		
		
	
	结构体类型为值类型，方法调用中遵守值类型传递机制，进行值拷贝
		func (cat Cat) eat() {
		    cat.Name = "robot"
		    fmt.Println(cat.Name)
		}
		func main() {
		    var c Cat = Cat{Name: "alice"}
		    c.eat()
		    fmt.Printf("c: %v\n", c)
		}
		//输出结果为robot
		//c: {alice 0 }
		
		
	如果在方法中修改结构体变量的值，可以通过结构体指针的方式处理
	
		func (c *Cat) eat_1() {
		    c.Name = "robot"
		}
		func main() {
		    var c Cat = Cat{Name: "alice"}
		    c.eat_1()
		    fmt.Printf("c: %v\n", c)
		}
		//输出结果为 c: {robot 0 }
		
	go中的方法作用在指定的数据类型上 自定义类型都可以有方法 int float32等都可以定义方法
		
		type integer int
		func (i integer) print() {
		    fmt.Print(i)
		}
		func main() {
		    var i integer = 10
		    i.print()
		}
	
	类型实现String()方法后，fmt.print()方法会默认调用类型的String()方法进行输出 （类似java中重写toString()方法) 前提是使用指针绑定
	
	结构体工厂模式 返回一个student的指针 相当于java中的构造方法 属性为私有时也需要编写get方法
		
		type student struct {
		    Name  string
		    Score float64
		}
		func Student(name string, score float64) *student {
		    return &student{
		        Name:  name,
		        Score: score}
		}
面向对象：
	继承： golang中没有extends 通过在结构体中嵌套父类匿名结构体实现继承特性
		结构体可以使用嵌套匿名结构体中所有字段和方法 无论公共私有属性/方法
		多层继承中同名方法遵循就近原则
		多继承时调用属性/方法时须明确指出父结构体
		
		type person struct {
		    name string
		    age  int
		}
		type student struct {
		    person
		    class string
		}

		
	类型断言：
		
		type Point struct {
		    x int
		    y int
		}
		func main() {
		    var null_interface interface{}
		    var point Point = Point{1, 3}
		    null_interface = point
		    var b Point
		    //类型断言 判断null_interface 是否为指向Point类型的指针
		    // 并且尝试转换 转换失败抛出异常
		    b = null_interface.(Point)
		    fmt.Println(b)
		}
		
		
