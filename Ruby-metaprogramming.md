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
扁平作用域，指避免使用作用域门， 使得变量可以互相访问的技术。
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

## 3.4 instance_eval() 
任何对象可以调 instance_eval方法 执行一个块。 这个对象会成为接收者self, 在块内。 传递给instance_eval()的块叫作**上下文探针**， instance_eval可以打破封装， 访问对象的实例变量。

### 洁净室
洁净室是一种类和对象，目的是为了执行块， 这种类通常提供了有用的方法供块来调用。

### 延迟执行

可以通过 Proc类把块变成一个对象，然后再执行
```ruby
inc = Proc.new { |x| x+1}
inc.call(2) #=> 3
```
或者可以通过内核方法 lambda 来生成一个 Proc对象，注意lambda比Proc更为通用。

```ruby
dec = lambda { |x| x-1} 
dec.class #=> Proc
```

块就像是方法的匿名参数，但是不方便把块传给另一个方法，或者保存成Proc，可以用&操作符
```ruby
def test
  yield
end

def metatest(a,b,&operation)
  test(&operation)
end

metatest(2,3) { puts 'hello'}
```
&操作符，在参数时，用来接受一个块， 其他时候，后面的变量是一个Proc对象， 可以加&作为块使用。也可以把&去掉， 用Proc#call()来调用。

### 对象方法分离
对象的方法可以用object#method()方法获取。 得到一个Method对象， 和Proc/lambda的作用域 （定义作用域，闭包） 不同， 它会在他自身所在的对象作用域执行。

# 类定义

## 当前类
类只是增强的模块。
**在类定义时，类本身充当了当前对象self ， 同理，也有当前类（模块）， 当定义方法时，该方法变成当前类的一个实例方法**
在顶级作用域定义方法时， 当前类是Object（main的类）， 所以定义的是Object Class的实例方法

class_eval 可以在不知道类名的情况下打开类，并定义方法
```ruby
def add_method_to(a_class)
  a_class.class_eval do
    def m; 'hello'; end
  end
end
```

* 和instance_eval不同， instance_eval仅修改self, class_eval同时修改了self，和当前类
* class_eval和Class不同在，可以使用小写字母变量，使用了扁平作用域

## 类实例变量
看下面例子,非常重要，容易和java搞混
```ruby
class MyClass
  # Class MyClass 实例变量, 此时self 是Class MyClass
  @my_var = 1

  def self.read; @my_var; end
  def write; @my_var =2; #生成对象的实例变量 
  end
  def read; @my_var; end 
end
```
**避免使用 @@类变量， 尽量使用上面的类实例变量**

## 4.3 单件方法
ruby允许给单个对象添加方法。
```ruby
str = "abc"
def str.title?
  self.update == self
end
# 检查单件方法
str.singleton_methods
```
类方法其实也是单件方法（类也是对象）.  def self.method() 等于 def Class.method()
## 类宏
写getter和setter：
```ruby
class MyClass
  def my_att=(value); @my_att=value; end
  def my_att; @my_att; end
end
```
Module#attr_ 可以定义访问方法 attr_reader() , attr_writer(), attr_accessor()
```ruby
class MyClass
  attr_accessor :my_att
end
```
在类宏中，经常会使用send, Object#send(method,xargs,&block)


## 4.4 EigenClass 单件类
单件类和单例没有关系，它是一个class,但只有一个实例，而且不能被继承，他是一个对象的单件方法的存活之所，其实就是eigenclass的实例方法。
获得eigenclass 作用域的操作
```ruby
class << an_obj
 #代码
end
#例子,来获取enginclasss
obj = Object.new
eigenclass = class << obj
  self
end
```
instance_eval 其实会修改self，和接受者的eigenclass
定义类方法
1. 定义在类上
```ruby
class MyClass
  def self.my_method; end
end
```
2. 定义在eigenclass 上
```ruby
class MyClass
  class << self
    def my_method; end
  end
end
```

方法查找路径：
（1） 对象
（2） 对象的eigenclass  找单件方法
（3） eigenclass 的superclass 是该对象的类D (假设 D < C)
(4) 类的实例方法在类D 里面
（5） singleton方法在类D 的eigenclass 里面
（6） eigenclass的超类是eigenclass

### 类属性
```ruby
class MyClass
  class << self
    attr_accesssor :c
  end
end

MyClass.c = 'it works'
```

### 类扩展
```ruby
module MyModule
  def my_method; 'hello'; end
end

class MyClass
  class << self
    include MyModule
  end
end
MyClass.my_method
```

### 用 Object#extend() 进行类扩展和实例扩展
extend是在接受者的eigenclass中包含模块的快捷方式
```ruby
obj = Object.new
obj.extend MyModule

class MyClass
  extend MyModule
end

```

## 4.6 别名

**alias 是一个关键字**，可以给Ruby方法取一个别名.

```ruby
class MyClass
  def my_method; 'hello'; end
  alias :m, :my_method
end
```

### 环绕别名
**如果先给一个方法命名一个别名，然后重定义它，则别名方法然会使用之前的定义.** 环绕别名的步骤
1. 给方法定义一个别名
2. 重新定义这个方法
3. 在新的方法中调用老的方法
问题：
1. 环绕别名仍是猴子补丁
2. 永远不该把环绕别名加载两次。

# 5.编写代码的代码

# 7 ActiveRecord 的设计

Kernel#autoload() 方法跟 模块名和文件名， 次一次引用该模块时，才导入文件
```ruby
module ActiveRecord
  autoload :Base, 'active_record/base'
  ...
end
```