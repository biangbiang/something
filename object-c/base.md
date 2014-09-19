object-c 入门基础篇
===================

### 一、Objective-C与C的渊源 

Objective-C诞生于 20 世纪 80 年代，由Brad Cox 发明，意在将流行的、可移植的 C 语言与优雅的 Smalltalk 语言结合在一起。Objective-C 是 C 语言的一个扩展集，它以 C 语言为基础,在语言中添加了一些微妙但意义重大的特性。 

苹果公司的iphone平台采用Objective-C做为native language的开发，Objective-C的内核是C语言的，并基于C语言实现OOP的一些特性。Objective-C是对C语言的扩展，这和C++的前身Better-c有很大的相似之处，Objective-C的新版本里实现了和Java类似的垃圾回收机制,但基于iphone平台的资源限制，iphone平台并不支持垃圾回收机制。 



### 二、初识Objective-C 

1、Cocoa的组成 

苹果公司将Cocoa、Carbon、QuickTime和OpenGL等技术作为框架集提供Cocoa组成部分有:

* (1)Foundation框架(有很多有用的，面向数据的低级类和数据结构)；
* (2)Application Kit（也称AppKit）框架（包含了所有的用户接口对象和高级类，例如NS……)，还有一个支持框架的套件，包括Core Animation和Core Image。 

2、NSLog相当于printf() 

	NSLog(@"hello Objective-C"); 

	//注：@是Objective-C在标准C语言基础上添加的特征之一，双引号的字符串前面有一个@，这表示引用的字符串应该作为Cocoa的NSString元素处理 

	NSLog(@"are %d and %d different? %@",5,5,boolString(areTheyDifferent)); 

	//注意%@：使用NSLog输出任何对象值时，都会使用这个格式说明 

3、BOOL使用8位存储，YES定义为1，NO定义为0，大于1不为YES，跟标准C不同。 

若不小心将一个长于1字节的整型值赋给BOOL，则只截取低八位 

Obejective-C中1不等于1，绝对不要将BOOL值和YES比较 



### 三、怪异的Objective-C语法结构 

我这里之所以说它的怪异，是相对于习惯其它编程语言(JAVA，C++等语言)，下面把它最常见的特色简单说明： 

#### 1、减号（或者加号） 

我们每天都会在IOS开发中见到在方法名前“+”或“─”号，那么这些是什么，怎么这么奇怪？ 

首先要把以前学习面向对象语言的惯性思维抛去，在Objective-C里面没有public和private的概念，你可以认为全是public；它只有类方法和实例方法，加号表示类方法，类方法可以直接调用，而不用创建这个类的实例；减号表示实例方法，需要创建这个类的实例对象才可以调用。 

比如c#/java中，一个方法的写法可能是： 

	private void hello(bool ishello) 
	
	{ 
	
	//OOXX 
	
	} 



用Objective-C写出来就是 

	-(void) hello:(BOOL)ishello 
	
	{ 
	
	//OOXX 
	
	} 

有过编程经验的人，理解这些应该不难！ 

#### 2、中括号 

中括号可以认为是如何调用你刚才写的这个方法，通常在Objective-C里说“消息”。 

比如C#里你可以这么写： 

	this.hello(true); 

在Objective-C里，就要写成： 

	[self hello:YES]; 

#### 3、NS**** 

在IOS开发中，经常会遇到NS开头的对象，这个要从乔帮主历史恩怨说起。当年Steve Jobs 和John Scullery与恩怨，乔帮主当年被人挤兑出苹果，自立门户的时候做了个公司叫做NextStep，里面这一整套开发包很是让一些科学家们喜欢，而现在Mac OS用的就是NextStep这一套函数库。 

这些开发NextStep的人们比较自恋地把函数库里面所有的类都用NextStep的缩写打头命名，也就是NS****了。比较常见的比如： 

	NSLog 

	NSString 

	NSInteger 

	NSURL 

	NSImage 

	… 

你会经常看到一些教学里面会用到： 

	NSLog (@"%d",myInt); 

这句话主要是在console里面跟踪使用，你会在console里面看到myInt的值（在XCode里面运行的时候打开dbg窗口即可看到）。 

你还可以看到其他名字打头的一些类，比如CF、CA、CG、UI等等，比如 

CFStringTokenizer 这是个分词的东东 

CALayer 这表示Core Animation的层 

CGPoint 这表示一个点 

UIImage 这表示iPhone里面的图片 

CF说的是Core Foundation，CA说的是Core Animation，CG说的是Core Graphics，UI说的是iPhone的User Interface……还有很多别的，等你自己去发掘了。 

#### 四、Objective-C常见语法说明 

1. 头文件引用使用 #import “文件名”或者 #import <文件名>的形式以确保每个头文件仅被包含一次； 

2. 类声明以 @interface 类名:继承类 开头，以 @end 结尾，类实现以@implementation 类名 开头，以 @end 结尾； 

3. 实例方法，即成员方法，在方法名前面添加一个减号(-)；类方法，在方法名前面添加一个加号(+)； 

4. 类方法的调用格式为 [类名 类方法]，成员方法调用格式为 [实例名 实例方 法]，这种模式在ObjC中被称为消息机制，[对象 消息]即给对象发送了一个消息，产生的 效果就是该对象调用了该类中定义的对应的实例方法； 

5. 下面以一个简单的例子来说明上述语法: 

Print类.h文件（声明文件） 

	#import <Foundation/Foundation.h> 

	@interface Print : NSObject { //Objective-c的所有类都继承于NSObject 

		// 成员属性 

		NSString *caption; 

		NSString *photographer; 

	} 

	//在Objective-C 2.0引入了属性合成，相当于之前的get/set方法 

	@property (nonatomic, copy) NSString *caption; 

	@property (nonatomic, copy) NSString *photographer; 

	// 类方法 

	+ (NSString*)printName; 

	@end 

Print类.m文件（实现文件） 

	#import "Print.h" 

	@implementation Print 

	@synthesize photographer; 

	@synthesize caption; 

	// 类方法 

	+ (NSString*)printName 

	{ 

		return (@"Print Result"); 

	} 



	@end 

Print 类使用 

	#import "Print.h" 

	int main(int argc, const char *argv[]) 

	{ 

		NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init]; 

		return NSApplicationMain(argc, (const char **)argv); 

		// 类方法调用 

		NSLog(@"ClassName = /"%@/"", [Print printName]); 



		// 初始化 

		Print* p = [[Print alloc] init]; 

		// 设置器调用 

		[p setCaption:@"MyCaption"]; 

		[p setPhotographer:@"MyPhotographer"]; 



		// 获取器调用 

		NSLog(@"Caption = /"%@/"", [p caption]); 

		NSLog(@"Photographer = /"%@/"", [p photographer]); 

		[pool drain]; 



		return 0; 

	} 

运行这段代码，结果如下: 

	2011-05-31 11:12:13.715 demo[1471:903] ClassName = "Print Result" 

	2011-05-31 11:12:13.718 demo[1471:903] Caption = "MyCaption" 

	2011-05-31 11:12:13.718 demo[1471:903] Photographer = "MyPhotographer

