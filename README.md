# openwrt-luci-lua
公司做路由器，写登陆操作页面，openwrt框架，luci，lua语言写，学习笔记
openwrt   uhttpd
lua
1. 添加mymodule模块：module(“luci.controller.myapp.mymodule”, package.seeall) 
2. 注册system节点：entry(path,target,title,order) 
  A、函数解析
  在controller目录下，每个.lua文件中，都有一个index()函数，其中主要调用entry()函数；
  
    1）path：插入的节点路径,如：{“admin”, “system”, “heyg1”},表示在admin下system中插入节点heyg1
    
    2）target：主要有：alise、firstchild、call、cbi、form、template。 
    
可以分成两类，前两种主要用于链接其它node，后几个个则是主要的操作、以及页面生成。 

template方式：即调该节点会直接调用view下的相应htm文件,举例：template(“heyg/heyg1”)会调用view/heyg/heyg1.htm文件 

cbi/form方式：会调用model下的相应文件做相应的处理 

call方式：会调用本文件或者导入文件的函数

   3）title：插入节点在对应位置的名字，在web界面对应菜单中的显示名字，写为_(“heyg1”)格式更符合
   
   4）order：插入结点的同等级的不同分类，或者说是区别同等级下的其他结点的数字代号，级别是从小到大
   
  B、函数属性解析
  
对于插入一个结点，该结点除了有相应的名称和处理动作之外，它还有一些相应的属性，我们可以手动的设置它的属性值类似于entry().dependent=false 

官方给出以下属性：

    1）dependent :当该节点的父节点丢失时，将该节点保护起来，不让它被意外调用
    
    2）leaf:如果该节点下还有其他子节点，解析到该结点时，就不向下继续解析其子节点。
    
    3）sysauth：在使用该节点时需要一个系统账户验证
    
    4）I18n：定义了当求页面请求时，哪些文件会自动加载
    
3、luci.http.formvalue()

  当我们点击一个按键时候，luci可以实现运行一个应用程序或者脚本
  
  luci.util.exec()
  
  该函数也是一次只能执行一条Linux命令，也可以先加载类库，如下：
  
  local util = require 'luci.util'; util.exec("mkdir /data/test")
  
  luci.dispatcher.build_url()
  
  LUCI的跳转方式、调用HTTP处理类中的重定向函数redirect()来进行跳转，而为了URL格式规范，所以这里使用了基础类中的build_url()函数来生成规范的URL路径。
  
  luci.http.redirect()
  
  luci的http路径重定向
  
  luci.template.render()
  
  luci模板渲染
  
  io.popen ([prog [, mode]])
  
  使用这个函数可以调用一个命令（程序），并且返回一个和这个程序相关的文件描述符，一般是这个被调用函数的输出结果，这个文件打开模式由参数mode确定，有取   值"r"和"w"两种，分别表示以读、写方式打开，默认是以读的方式。
  
  从运行的结果来看，使用这个函数来执行命令，和直接在命令行界面执行命令的结果上一样的，只不过使用函数这种方式的结果保存在文件中。
  
  luci.http.prepare_content()
  
  luci.http准备的内容
  
  index = true 表示总标题显示，alias方法为链接到xxx节点


％d整型输出，％ld长整型输出，

％o以八进制数形式输出整数，

％x以十六进制数形式输出整数，

％u以十进制数输出unsigned型数据(无符号数)。

％c用来输出一个字符，

％s用来输出一个字符串，

％f用来输出实数，以小数形式输出，（备注：浮点数是不能定义如的精度的，所以“%6.2f”这种写法是“错误的”！！！）

％e以指数形式输出实数，

％g根据大小自动选f格式或e格式，且不输出无意义的零。

scanf(控制字符，地址列表) 
格式字符的含义同printf函数，地址列表是由若干个地址组成的表列，可以是变量的地址，或字符串的首地址。如scanf("％d％c％s",&a,&b,str)；

luci中view的html引用css，js方法是<script type="text/javascript" src="<%=resource%>/cbi.js"></script>//<link rel="stylesheet" type="text/css" media="screen" href="<%=media%>/cascade.css" />
