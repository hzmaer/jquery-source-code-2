# jquery源码面向对象程序设计

首先看61行这段代码

jQuery=function(selector,context){

return new jQuery.fn.init(selector,context,rootjQuery)

};

分析：为什么要这样写?

一：首先来看普通面向对象程序的写法

假设有构造函数
             function Cc(){

             }
这个构造函数的原型 

               初始化方法：Cc.prototype.init=function(){};
 
               css方法（简单举个例子）Cc.prototype.css=function(){};
                 
                 
调用方式：var cc=new Cc();//实例化创建一个对象
  
           cc.init();//初始化
           
           cc.css();
           
以上是面向对象程序的大体写法；
  
二：再来看看jquery的做法：
  
                function jQuery(){
               
                return new jQuery.fn.init();
                
                }
                
                jQuery.prototype.init=function(){};
 
                jQuery.prototypee.css=function(){};
                
 调用方式 jQuery().css();
 
 解析运行原理：jQuery()运行后，返回new jQuery.fn.init()（说明new jQuery.fn.init()为构造函数）,相当于省略掉了初始化步骤，
 
 所以接下来可以直接运行jQuery的css()方法；
 
 隐藏的问题：返回new jQuery.fn.init()（说明new jQuery.fn.init()为构造函数）后为什么 jQuery就能直接调用css()方法呢？
 
 解析：请看jquery源码第283行 jQuery.fn.init.prototype = jQuery.fn;
 
 由于fn就是原型（96行有一行代码jQuery.fn=jQuery.prototype,把jquery的prototype赋值给jQuery的fn,说明prototype就是fn），
 
 所以上面一行代码等价于  jQuery.prototype.init.prototype = jQuery.prototype;
 
 这行代码表示把jQuery的原型赋给了jQuery.prototype.init初始化构造函数的原型，这样就形成了一个对象的引用关系，所以在后面的
 
 构造函数（jQuery）原型基础上进行修改等价于在前面构造函数（jQuery.prototype.init）原型上进行修改，所以在jQuery.prototype
 
 下写的方法可以供jQuery.prototype.init来使用。
             
             
  
  
  
           
           
            
            
     
