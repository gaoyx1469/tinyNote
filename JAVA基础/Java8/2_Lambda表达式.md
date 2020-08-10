#Lambda表达式

1. lambda是一个匿名函数，可以理解为一段可传递的代码

2. lambda语法  
    引入箭头操作符 ->，将Lambda表达式拆分为两部分，左侧是参数列表，右侧是需要执行的功能  
    Lambda表达式需要函数式接口的支持    
    格式为(参数类型及形参，可有多个) -> {方法块}  
        参数类型可省略  
        只有一个形参且省略了参数类型时，()也可省略 
    
    函数式接口--对于只有一个抽象方法的接口，需要该接口的对象时，就可以使用lambda表达式  
    方法引用--lambda表达式的格式简化(this和super也可算是object)  
        * object :: instanceMethod	如 x->System.out.println(x)可写成System.out::println   
        * Class ::　staticMethod		如(x,y)->Math.pow(x,y)可写成Math::pow  
        * Class　:: instanceMethod		如(x,y)->x.compareToIgnoreCase(y)可写成String::compareToIgnoreCase  
    构造器引用--lambda表达式的格式简化    
        * Class :: new	如x->new int[x]可写为int[]::new  
    变量作用域  
        * lambda表达式可引用（捕获）实际上的最终变量，即变量初始化后就不赋新值了
      