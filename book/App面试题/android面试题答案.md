1. 接口和抽象类有什么区别  
> 一个类只能继承一个抽象类；但是一个类可以实现多个接口。接口是定义混合类型的理想选择。  
抽象类除了不能被实例化之外，它和普通Java类没有任何区别。可以定义变量和实现默认方法，而接口虽然在Java8中引入了默认方法，但其不能定义变量。  
接口的方法和Field默认修饰符是public，不可以使用其它修饰符。  
接口允许我们构造非层次结构的类型系统，接口是定义混合类型的理想选择。


2. 简述android的4种启动模式及特点。
> standard: 
每次新建一个activity实例,并加入栈中。
场景：普通的页面使用
singleTop: 
每次扫描栈顶，如果在任务栈顶发现了相同的实例则重用，否则新建并压入栈顶。
场景：适用于整个App只有单一页面展示
singleTask: 
与singleTop的区别是singleTask会扫描整个任务栈并制定策略。
场景： 适用于主界面
singleInstance: 
每次启动新建一个栈,并且该栈中只有它自己一个实例
场景：单独的一个展示界面。

3. 简述单线程模型中Message、Handler、Message Queue、Looper之间的关系。
> 首先，是这个MessagQueue，MessageQueue是一个消息队列，它可以存储Handler发送过来的消息，其内部提供了进队和出队的方法来管理这个消息队列，其出队和进队的原理是采用单链表的数据结构进行插入和删除的，即enqueueMessage()方法和next()方法。这里提到的Message，其实就是一个Bean对象，里面的属性用来记录Message的各种信息。  
然后，是这个Looper，Looper是一个循环器，它可以循环的取出MessageQueue中的Message，其内部提供了Looper的初始化和循环出去Message的方法，即prepare()方法和loop()方法。在prepare()方法中，Looper会关联一个MessageQueue，而且将Looper存进一个ThreadLocal中，在loop()方法中，通过ThreadLocal取出Looper，使用MessageQueue的next()方法取出Message后，判断Message是否为空，如果是则Looper阻塞，如果不是，则通过dispatchMessage()方法分发该Message到Handler中，而Handler执行handlerMessage()方法，由于handlerMessage()方法是个空方法，这也是为什么需要在Handler中重写handlerMessage()方法的原因。这里要注意的是Looper只能在一个线程中只能存在一个。这里提到的ThreadLocal，其实就是一个对象，用来在不同线程中存放对应线程的Looper。  
最后，是这个Handler，Handler是Looper和MessageQueue的桥梁，Handler内部提供了发送Message的一系列方法，最终会通过MessageQueue的enqueueMessage()方法将Message存进MessageQueue中。我们平时可以直接在主线程中使用Handler，那是因为在应用程序启动时，在入口的main方法中已经默认为我们创建好了Looper。


4. 简述开发中使用的模式及特点。(MVC | MVP | MVVM)
> MVC:  
M、V、C之间单向通信。简单，容易理解，Controller 提高了应用程序的灵活性和可配置性。Controller 可以用来连接不同的 Model 和 View 去完成用户的需求，也可以构造应用程序提供强有力的手段。给定一些可重用的 Model 、 View 和Controller 可以根据用户的需求选择适当的 Model 进行处理，然后选择适当的的 View 将处理结果显示给用户。但在Android中使用Activity做为Controller容易导致Controller和View耦合在一起。  
MVP:  
M、V、P之间双向通信。
View 与 Model 不通信，都通过 Presenter 传递。  
Presenter完全把Model和View进行了分离，主要的程序逻辑在Presenter里实现。  
Presenter与具体的View是没有直接关联的，而是通过定义好的接口进行交互，从而使得在变更View时候可以保持Presenter的不变，这样就可以重用。不仅如此，还可以编写测试用的View，模拟用户的各种操作，从而实现对Presenter的测试–从而不需要使用自动化的测试工具。  
MVVM:  
ViewModel是暴露公共属性和命令的视图的抽象。MVVM没有MVC模式的控制器，也没有MVP模式的presenter，有的是一个绑定器。在视图模型中，绑定器在视图和数据绑定器之间进行通信  
MVVM模式试图获得MVC提供的功能性开发分离的两个优点，同时利用数据绑定的优势和通过绑定数据的框架尽可能接近纯应用程序模型。它使用绑定器、视图模型和任何业务层的数据检查功能来验证传入的数据。结果是模型和框架驱动尽可能多的操作，消除或最小化直接操纵视图的应用程序逻辑（如代码隐藏）。


5. 自定义一个ViewGroup, 使其子View以田字结构排列，简述其主要步骤。 
> 覆写generateLayoutParams 和 generateDefaultLayoutParams 使ChildView设置的margins可以生效。  
 覆写onMeasure方法 计算所有ChildView的宽度和高度 然后根据ChildView的计算结果，设置自己的宽和高  
 注1 ViewGroup中测量child一定要用measureChildWithMargins而不是measureChild。  
 注2 ViewGroup的宽高不能简单的对两边的ChildView相加取最大值，这样会导致ChildView重叠  
 覆写onLayout方法对所有ChildView进行定位（设置ChildView的绘制区域）遍历所有的ChildView，根据ChildView的宽和高以及margin，然后分别将0，1，2，3位置的childView依次设置到左上、右上、左下、右下的位置。



6. 简述HashMap的实现。
> 通过hash的方法，通过put和get存储和获取对象。
存储对象时，将K/V传给put方法时，它调用hashCode计算hash从而得到bucket位置，进一步存储，HashMap会根据当前bucket的占用情况自动调整容量(超过Load Facotr则resize为原来的2倍)。获取对象时，我们将K传给get，它调用hashCode计算hash从而得到bucket位置，并进一步调用equals()方法确定键值对。如果发生碰撞的时候，Hashmap通过链表将产生碰撞冲突的元素组织起来，在Java 8中，如果一个bucket中碰撞冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。

7. Android源码中使用了哪些设计模式，试举几例
> 1. 观察者模式: BaseAdapter、EventBus、RxJava
> 2. 模板方法模式：AsyncTask、Ativity生命周期
> 3. 访问者模式：ButterKnife
> 4. Builder模式
> 5. 适配器模式: Adapter
> 6. 工厂模式
> 7. 单例模式



8. 是否熟悉Kotlin语言，比较Java与Kotlin异同。
> 相同点：都是基于JVM的编程语言
 Kotlin是一种更加现代化的语言,被誉为android界的Swift。
> Kotlin相对于Java:
> 
> 1. 它更加易表现:这是它最重要的优点之一。你可以编写少得多的代码。
> 2. 它更加安全:Kotlin是空安全的
> 3. 它是函数式的
> 4. 它可以扩展函数
> 5. 语法层面：常量、变量、静态变量的定义、with、apply、when


