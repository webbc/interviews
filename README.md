# Interview

分享Golang、算法、数据结构、网络、操作系统、分布式等面试题

## Golang

### 基础

- 1、JSON标准库对nil slice 和空slice的处理是一致的吗？

   不一致。对nil slice解析出来的是个null，空slice解析是一个空json数组<br>
   nil slice：只是申明了slice，但未初始化<br>
   空 slice：申明且已经初始化的slice，但还未使用<br>
   
- 2、golang中引用类型有哪些？

   只有三种引用类型：切片slice、字典map、管道channel

- 3、golang数组不是引用类型吗？

   golang的数组不是引用类型，是值类型，所以将一个数组赋值给另外一个数组或者在方法传数组类型的参数是会复制一份，增加了内存拷贝。所以最好用数组元素最好存指针，就是指针数组，这样就不会出现拷贝了。

   golang的数组类型包括数组长度，所以[2]int和[3]int不能进行赋值，[2]int和[2]string更不能赋值。

   ```
   var arr1 = new([3]int)	arr1 的类型是 *[3]int
   var arr2 [3]int			arr2 的类型是 [3]int	
   ```

- 4、slice的数据结构是什么样的？

   slice的数据结构主要是数组的指针、长度、容量3个字段，元素存储还是依赖数组，只是slice存了一个数组的指针。

   ``````
   // 使用make来创建
   a := make([]uint32,5)  		创建容量和长度都是5的slice
   a := make([]uint32,0,5) 	创建容量是5、长度是0的slice，长度是不能超过容量的
   
   // 赋值初始化
   a := []int{10, 20, 30, 40}  创建容量和长度都是4的slice
   a := []string{99: ""}    	创建容量和长度都是100的slice
   
   // 申明
   var a []int    				创建nil的slice，长度和容量是0，数组指针是nil
   ``````

- 5、切片的扩容

   每次使用append()函数，肯定会增加切片的长度，如果容量不够了，就需要进行扩容，函数 append() 会智能地处理底层数组的容量增长。

   在切片的容量小于 1000 个元素时，总是会成倍地增加容量。一旦元素个数超过 1000，容量的增长因子会设为 1.25，也就是会每次增加 25%的容量。

   扩容会将源slice数据拷贝到新slice指向的数组中。

- 6、切片的切割

   ``````
   slice[i:j] 
   slice[i:j:k]
   ``````

   i 表示从 slice 的第几个元素开始切，j 控制切片的长度(j-i)，k 控制切片的容量(k-i)，如果没有给定 k，则表示切到底层数组的最尾部。

   需要注意新的切片和老切片共享一个底层数组。

   ``````
   a := []int{10, 20, 30, 40}
   b := fruit[2:3:3]
   // 向b追加新数值
   b = append(b, 50)
   ``````

   内置函数 append() 在操作切片时会首先使用可用容量。一旦没有可用容量，就会分配一个新的底层数组。如果在创建切片时设置切片的容量和长度一样，就可以强制让新切片的第一个 append 操作创建新的底层数组，与原有的底层数组分离。我们限制了b的容量和长度都是 1，当我们第一次对 b调用 append() 函数的时候，会创建一个新的底层数组，这个数组包括 2 个元素，首先会将30赋值过来，再追加新的50，并返回一个引用了这个底层数组的新切片，因为新的切片b拥有了自己的底层数组，这样就不会影响到其他的切片了。

- 7、切面笔试题

   ``````
   fruit := []string{"Apple", "Orange", "Plum", "Banana", "Grape"}
   myFruit := fruit[2:3:3]    // 创建一个新切片，长度是1、容量也是1
   fmt.Println(myFruit)
   fmt.Printf("%p,%v\n", fruit, fruit)      
   fmt.Printf("%p,%v\n", myFruit, myFruit)
   
   myFruit = append(myFruit, "12")  // 因为新的切片容量是1，append的时候回自动创建一个新的底层数组
   fmt.Printf("%p,%v\n", fruit, fruit)
   fmt.Printf("%p,%v\n", myFruit, myFruit) 
   
   myFruit[0] = "xxxx"
   fmt.Printf("%p,%v\n", fruit, fruit)   
   fmt.Printf("%p,%v\n", myFruit, myFruit) // 因为是一个新的底层数据，不会影响到老的切片
   
   // 输出
   [Plum]
   0xc00003c050,[Apple Orange Plum Banana Grape]
   0xc00003c070,[Plum]
   0xc00003c050,[Apple Orange Plum Banana Grape]
   0xc000004560,[Plum 12]
   0xc00003c050,[Apple Orange Plum Banana Grape]
   0xc000004560,[xxxx 12]
   ``````
   
 - 8、make和new的区别？
 
 make关键字只能用来初始化map、slice和chan类型变量
 new关键字是根据传入的类型分配一片内存空间并返回指向这片内存空间的指针

## 算法&数据结构


## 网络


## 操作系统


## 分布式
