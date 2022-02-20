## 1.2 打开类
指打开基础类,或特定的类的定义，加上特定的通用方法
```ruby
class String
    def to_alphanumeric
        gsub /[^\w\s]/,''
    end
end
```
问题是， 容易覆盖类原有的方法，这叫 **猴子补丁**
### tips
搜索方法关键词
```ruby
[].methods.grep /^re/
```
查看instance variables => Object#instance_variables()

**实例变量属于object, 但是方法都在类中（实例方法）**
**类自身也是对象 ，它的类是class, class的超类是module, module的超类是object, object的类是class, class的类是它自身** 
**class比module多了new(),allocate,superclass()三个方法**
下文用#表示 实例方法 Class.method 表示类方法 (其实就是他的超类的实例方法)

### tips
查看实例方法（非继承）
```ruby
inherited = false
Class.instance_methods(inheried)

```

## 常量
任何已大写字母开头的 （类名和模块名） 都是常量
* 常量可以用树形结构 :: 表示  ：：M：：Y
* Module#constants() 返回范围内常量
* Module.nesting() 返回当前常量路径
**通常自定义的类外面需要module来创建一个namespace**

### tips
单行写法 ;可以替代\n
```ruby
class A; end
```

## 1.5调用方法
1. 接受者， 方法调用的obj
2. 祖先链， 找到class, 然后superclass(包含module) 

### tips
得到祖先链
```ruby
MySubClass.ancestors()
```
3. 类可以 include 一个module, Ruby创建了一个封装该模块的匿名类，在这个类的祖先链正上方, 但这类include不太好在ancesters里面找到
```ruby
class C
    include M
end
```
4. Kernal就是一个模块， 有print(), Object include 了 Kernal. 内核方法

## 执行方法
被调用方法， 通过self 来找到接受者， 所有实力变量均来自self.
1. 程序开始的self 叫main, 顶级上下文
2. 类/模块定义中，self 即是类和模块本身object,可以用来定义类变量
3.  当没有明确定义接受者的调用， 调用都是self 的方法 ； private 方法必须被self (也就是那个类) 调用

# 代码块

##块的复习
1. 块可以由 do | x,y| end 给出， 或者 一行内可以由括号给出 { | x, y| x+y } 
2. 只有调用方法可以 定义一个块， 块会传递给一个方法， 方法用yield来回调这个块
3. 方法内， 可以用 Kernal#block_give?() 这个内核方法检测是否有 给出的块

### tips 定义全局方法
如果要定义全局方法，可以用打开类写在 module Kernel 里面
```ruby
#ruby的final怎么写, 用ensure
begin
    yield
ensure
    resource.dispose
end
```

## 3.3 闭包
块是浮动代码，需要 **局部变量，实例变量，self， 这些称为绑定**

**块的绑定，来自于它定义时看到的，这个称为闭包**
块会在定义时获取周围的绑定，你可以在块的内部定义额外的绑定，不过这个绑定会在块结束时消失。

**某行代码可见的变量，绑定， 常量，方法，称为作用域**

Kernel#local_variables()可以查看当前局部变量

**程序会在3个时候关闭前一个作用域，打开一个新作用域， 1.类定义， 2. 模块定义， 3. 方法 这成为作用域门**

其实作用域门就是 class,module,def, 三个关键字， class/module内的代码会被立即执行, def会在调用时执行

### tips 全局变量和顶级实例变量
```ruby
$var #始终可以全局访问

# @var是main的顶级实例变量， 在main 是self的时候可以访问
@var = "abc"
def my_method
  @var
end
```

## 扁平作用域 （穿越作用域门）
可以用以下方法替代class关键字
```ruby
my_var = "hello"
# 用Class.new替代class关键字， 同理，Module可以用Module.new()来替代
MyClass = Class.new do 
  puts "#{my_var}"
  # 用 Module#define_method()替代def
  define_method :my_method do
     puts "#{my_var}"
  end 
```