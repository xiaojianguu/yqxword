1. 一个objc对象如何进行内存布局？（考虑有父类的情况）
> 1. objc对象属性的具体值保存在实例对象中。
> 2. 每个对象都有一个ISA指针和superclass指针，实例对象的ISA指针指向类对象，类对象的ISA指针指向元类对象，根元类对象的ISA指针指向自己。类对象的superclass指针指向父类对象，元类对象的superclass指针指向父元类对象，根元类对象的ISA指针指向根类对象。
> 3. 类对象中包含了ISA指针，superclass指针，属性数组，协议数组，属性方法缓存数组，属性方法数组等
> 4. 元类对象中包含了ISA指针，superclass指针，属性数组，协议数组，类方法缓存数组，类方法数组等


2. 说说对copy与mutableCopy的理解？
> 1. 对于不可变对象，copy都是浅复制，即指针复制。mutableCopy 都是Alloc一个新对象返回。
>2. 对于可变对象，copy和mutableCopy都是Alloc新对象返回。
>3. 不论是可变还是不可变对象，copy返回的对象都是不可变的，mutableCopy返回的对象都是可变的。
>4. 容器类对象，不论是可变的还是不可变的，copy，mutableCopy返回的对象里所包含的对象的地址和之前都是一样的，即容器内对象都是浅拷贝。
3. 说说对NSNotification、Block、Delegate和KVO的使用场景？
>1. KVO就是cocoa框架实现的观察者模式，一般同KVC搭配使用，通过KVO可以监测一个值的变化，比如View的高度变化。是一对多的关系，一个值的变化会通知所有的观察者。
>2. NSNotification是通知，也是一对多的使用场景。在某些情况下，KVO和NSNotification是一样的，都是状态变化之后告知对方。NSNotification的特点，就是需要被观察者先主动发出通知，然后观察者注册监听后再来进行响应，比KVO多了发送通知的一步，但是其优点是监听不局限于属性的变化，还可以对多种多样的状态变化进行监听，监听范围广，使用也更灵活。
> 3. delegate 是代理，就是我不想做的事情交给别人做。比如狗需要吃饭，就通过delegate通知主人，主人就会给他做饭、盛饭、倒水，这些操作，这些狗都不需要关心，只需要调用delegate（代理人）就可以了，由其他类完成所需要的操作。所以delegate是一对一关系。
> 4. block是delegate的另一种形式，是函数式编程的一种形式。使用场景跟delegate一样，相比delegate更灵活，而且代理的实现更直观。
> 5. KVO一般的使用场景是数据，需求是数据变化，比如股票价格变化，我们一般使用KVO（观察者模式）。delegate一般的使用场景是行为，需求是需要别人帮我做一件事情，比如买卖股票，我们一般使用delegate。Notification一般是进行全局通知，比如利好消息一出，通知大家去买入。delegate是强关联，就是委托和代理双方互相知道，你委托别人买股票你就需要知道经纪人，经纪人也不要知道自己的顾客。Notification是弱关联，利好消息发出，你不需要知道是谁发的也可以做出相应的反应，同理发消息的人也不需要知道接收的人也可以正常发出消息。

4. 讲一下对MVC、MVVM、MVP的理解？
> 1. MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。
特点：所有方式都是单向通信
> 2. mvp的全称为Model-View-Presenter，Model提供数据，View负责显示，Controller/Presenter负责逻辑的处理。MVP与MVC有着一个重大的区别：在MVP中View并不直接使用Model，它们之间的通信是通过Presenter (MVC中的Controller)来进行的，所有的交互都发生在Presenter内部，而在MVC中View会直接从Model中读取数据而不是通过 Controller。
>3. MVVM是Model-View-ViewModel的简写。微软的WPF带来了新的技术体验，如Silverlight、音频、视频、3D、动画……，这导致了软件UI层更加细节化、可定制化。同时，在技术层面，WPF也带来了 诸如Binding、Dependency Property、Routed Events、Command、DataTemplate、ControlTemplate等新特性。MVVM（Model-View-ViewModel）框架的由来便是MVP（Model-View-Presenter）模式与WPF结合的应用方式时发展演变过来的一种新型架构框架。它立足于原有MVP框架并且把WPF的新特性糅合进去，以应对客户日益复杂的需求变化。

5. iOS实现多线程有几种方式？
>	1）pthread
>
>	2）NSThread
>
>	3）GCD
>
>	4）NSOperation

6. 如果让你去实现一个网络图片加载的SDK，你有什么思路？
>	1. 判断链接是否有效（设置黑名单数组）
>	2. 判断内存是否有图片缓存
>	3. 硬盘查找图片是否已经缓存
>	4. 从硬盘读取到了图片，将图片添加到内存缓存中（如果空闲内存过小， 会先清空内存缓存）
>	5.共享或重新生成一个下载器 SDWebImageDownloader开始下载图片
>	6. 下载完成，回调给需要的地方展示图片
>	7. 将图片保存到 SDImageCache中，内存缓存和硬盘缓存同时保存。
>	8. 在内存警告或退到后台的时候清理内存图片缓存，应用结束的时候清理过期图片。设置缓存上限，按照日期倒序删除。

7. 说说对Block的理解？
>	1. Block是一个OC对象，他继承自NSObject
>	2. Blcok分为全局Blcok，堆Block，栈Block
>	3. 在 MRC下:只要没有访问外部变量，就是全局block。访问了外部变量，就是栈block。显示地调用[block copy]就是堆block。
>	4. 在 ARC下:只要没有访问外部变量，就是全局block。如果访问了外部变量，那么在访问外部变量之前存储在栈区，访问外部变量之后存储在堆区。
>	5. __block的作用:将外部变量的传递形式由值传递变为指针传递，从而可以获取并且修改外部变量的值。同样，外部变量的修改，也会影响block函数的输出。
>	6. block循环引用问题：当一个类的对象持有block，block里面又引用了这个对象，那么就是一个循环引用的关系。可以用strong-weak-dance的方法解除循环引用。

8. 简述OC中的消息转发机制？
>	当objc_msgSend方法调用找不到响应的函数名称时就会进行消息转发。
>	主要分为3步:
>
>	1. 动态方法解析 调用方法+(BOOL)resolveInstanceMethod:(SEL)sel(实例方法动态解析)和+ (BOOL)resolveClassMethod:(SEL)sel(类方法动态解析)。
>	2. 备援接收者 调用方法 - (id)forwardingTargetForSelector:(SEL)aSelector 
>	3. 完全转发 调用方法- (void)forwardInvocation:(NSInvocation *)anInvocation和- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector
	
9. iOS中常用的线程锁有哪些，分别具有哪些特点？
>	1. @synchronized 关键字加锁 互斥锁，性能较差不推荐使用
>	2. NSLock 互斥锁 不能多次调用 lock方法,会造成死锁
>	3. NSRecursiveLock递归锁，NSRecursiveLock类定义的锁可以在同一线程多次lock，而不会造成死锁。递归锁会跟踪它被多少次lock。每次成功的lock都必须平衡调用unlock操作。只有所有的锁住和解锁操作都平衡的时候，锁才真正被释放给其他线程获得。
>	4. NSConditionLock条件锁，顾名思义，这个锁对象有一个condition属性，只有condition的值相同，才能获取到该锁，并且执行解锁操作。
>	5. POSIX互斥锁，POSIX是Unix/Linux平台上提供的一套条件互斥锁的API。用法和特点与NSLock类似。
>	6. dispatch_semaphore信号量实现加锁，当信号量为0时，线程将会被卡死，通过信号量的增减来达到控制线程个数的目的。
>	7. OSSpinLock自旋锁，用法类似于NSLock，可以自动检查线程锁是否已经打开，效率比较高，但是被证明不是线程安全的。