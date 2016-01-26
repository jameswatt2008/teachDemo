# UIButton & NStimer
## 一、UIButton（按钮）
    、按钮风格(系统、自定义)
    、基本属性(标题、颜色、状态)
    、图片
    、添加事件及处理方法
    、UILabel和UIButton使用
###创建和使用UIButton对象###

- **buttonWithType**：类方法，在创建按钮时指定按钮的类型。

###UIButton常用属性和方法###

配置按钮标题的属性和方法：

- titleLabel属性：按钮标题的标签
- titleForState:方法：指定状态下的按钮标题
- setTitle:forState:方法：设置指定状态下按钮的标题
- titleColorForState:方法：指定状态下的按钮标题颜色
- setTitleColor:forState:方法：设置指定状态下按钮标题的颜色
- setTitleShadowColor:forState:方法：设置指定状态下按钮标题阴影的颜色

配置按钮显示的方法：

- backgroundImageForState:方法：获得指定状态下的背景图
- setBackgroundImage:forState:方法：设置指定状态下的背景图
- imageForState:方法：获得指定状态下的按钮图片
- setImage:forState:方法：设置指定状态下的按钮图片

获取按钮当前状态的属性：

- buttonType属性：可能的取值包括UIButtonTypeCustom、UIButtonTypeSystem、UIButtonTypeDetailDisclosure、UIButtonTypeInfoLight、UIButtonTypeInfoDark、UIButtonTypeContactAdd、UIButtonTypeRounedRect（过时）。
- currentTitle属性：按钮上当前显示的标题
- currentTitleColor属性：当前标题颜色
- currentImage属性：按钮上当前显示的图片
- currentBackgroundImage属性：按钮上当前显示的背景图片
- imageView属性：按钮上的图片视图

其他的属性和方法：

- addTarget:action:forControlEvents:方法：将为事件添加的消息接受者和对应的动作加入事件派发表，简而言之就是为控件绑定事件处理的回调方法
- removeTarget:action:forControlEvents:方法：与上面方法的作用相反
- enabled属性：控件是启动还是禁用
- state属性：控件所处的状态

###UIButton的常用事件和状态###

&emsp;&emsp;我们先说一下UIControl的所有可能的事件。

事件类型|说明
:--|:--
UIControlEventTouchDown|单点触摸按下事件，用户点触屏幕，或者又有新手指落下的时候
UIControlEventTouchDownRepeat|多点触摸按下事件，点触计数大于1：用户按下第二、三、或第四根手指的时候
UIControlEventTouchDragInside|当一次触摸在控件窗口内拖动时
UIControlEventTouchDragOutside|当一次触摸在控件窗口之外拖动时
UIControlEventTouchDragEnter|当一次触摸从控件窗口之外拖动到内部时
UIControlEventTouchDragExit|当一次触摸从控件窗口内部拖动到外部时
UIControlEventTouchUpInside|所有在控件之内触摸抬起事件
UIControlEventTouchUpOutside|所有在控件之外触摸抬起事件(点触必须开始与控件内部才会发送通知)
UIControlEventTouchCancel|所有触摸取消事件，即一次触摸因为放上了太多手指而被取消，或者被上锁或者电话呼叫打断
UIControlEventValueChanged|当控件的值发生改变时，发送通知。用于滑块、分段控件、以及其他取值的控件。你可以配置滑块控件何时发送通知，在滑块被放下时发送，或者在被拖动时发送
UIControlEventEditingDidBegin|当文本控件中开始编辑时发送通知
UIControlEventEditingChanged|当文本控件中的文本被改变时发送通知
UIControlEventEditingDidEnd|当文本控件中编辑结束时发送通知
UIControlEventEditingDidOnExit|当文本控件内通过按下回车键（或等价行为）结束编辑时，发送通知
UIControlEventAlltouchEvents|通知所有触摸事件
UIControlEventAllEditingEvents|通知所有关于文本编辑的事件
UIControlEventAllEvents|通知所有事件

&emsp;&emsp;对于UIButton来说，可能绝大多数处理的都是UIControlEventTouchUpInside事件，简单的说就是按钮点击的事件。

##UIImage简介##

&emsp;&emsp;UIImage是用来显示图像的对象。我们可以通过文件、接收到原始的数据来创建UIImage对象。

###创建UIImage对象的方式###

- **imageNamed:**：类方法，根据指定的文件名返回UIImage对象
- **imageWithData:**：类方法，根据指定的NSData对象创建UIImage对象
- **imageWithContentOfFile:**：通过文件加载指定路径下的内容获得UIImage对象

```Objective-C
	// 此种方式只能加载较小的图片
	UIImage *image1 = [UIImage imageNamed:@"abc"];

	NSString * strPath = [[NSBundle mainBundle]  pathForResource:@"abc" ofType:@"png"];
	// 该方式即使加载很大的图片不会使程序崩溃
    UIImage *image2 = [UIImage imageWithContentsOfFile:strPath];
   	
   	// 通过制定URL得到的数据创建图片对象
    UIImage *image3 =[UIImage imageWithData:[NSData dataWithContentsOfURL:[NSURL URLWithString:@"https://www.baidu.com/img/bdlogo.png"]]];
```




## 二、NSTimer（定时器）
    1、创建
    /*创建定时器
     1:时间间隔
     2:负责处理的对象
     3:定时做的事情
     4:一般写nil
     5:是否重复
     */
    _timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(changeColor) userInfo:nil repeats:YES];

    2、开启，创建后默认是开启的
    //将定时器的开启时间设置为很久以前的一个时间会启动定时器
    [_timer setFireDate:[NSDate distantPast]];

    3、暂停
    //将定时器的开启时间设置为将来的一个时间会关闭定时器
    [_timer setFireDate:[NSDate distantFuture]];

    4、销毁
    //定时器无需释放，但是需要让其失效
    if (_timer.isValid == YES) {
        [_timer invalidate];
        _timer=nil;
        //一定记得赋值为nil，每个对象添加到timer以后会引用计数增加，停用后设置nil可以让对象迅速释放 。
    }


## 练习题：
1、简单计时器
2、秒表，效果见图《StopWatch》
3、飞舞的🐷，效果见《FlutterPig》
