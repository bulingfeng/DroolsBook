

# DroolsBook

Drools规则引擎的中文书籍

## 目录

[1、Drools文件语法](Drl文件结构.md)

1. package

   用来标识drl文件的一个标识符，命名规范类似java文件，但是作用和java文件不太一样。只需要编写的drl文件中package不重名即可。

   ```drl
   package com.lingfeng.functionTest
   ```

2. import

   类似java的导入包，可以参考java的导包规则。

3. function

   顾名思义就是一个函数，可以类比java中得方法。需要注意的是只要与drl文件的package一致就可以引用其中的function。当然也可以放同一个drl文件中

   ```drl
   package com.lingfeng.functionTest
   
   import com.lingfeng.rules.languagereference.entity.Function
   
   function String hello(String applicantName) {
       return "Hello " + applicantName + "!";
   }
   
   rule "Using a function"
     when
       $function:Function(ifFunction == true)
     then
       System.out.println( hello( "this is a function" ) );
   end
   ```

4. query

   query就是按照条件进行查询。

   ```drl
   query "people under the age of 21"
       $person : Person( age < 21 )
   end
   
   QueryResults results = ksession.getQueryResults( "people over the age of 30" );
   System.out.println( "we have " + results.size() + " people over the age  of 30" );
   
   System.out.println( "These people are under the age of 21:" );
   
   for ( QueryResultsRow row : results ) {
       Person person = ( Person ) row.get( "$person" );
       System.out.println( person.getName() + "\n" );
   }
   ```

   Note:官方Person person = ( Person ) row.get( "person" );没有加$符号。必须保证别名的一致性。按照上述的是正确的代码。
 
 5. declare
 
 6. Global variables
 
 7. [attributes语法点击查看](rule_attributes.md)
 
 8. conditions
 ### 注意事项
    1. 如果javaBean中没有age这个属性，那么会调用getAge这个方法
    
    ```drl
    Person( age == 50 )
    
    // This is the same as the following getter format:
    
    Person( getAge() == 50 )
    
    2. 和上面类似。下面是转换关系。
    Person( address.houseNumber == 50 )
    
    // This is the same as the following format:
    
    Person( getAddress().getHouseNumber() == 50 )
    
    3. 如果age是number类型，但是比较的是string类型，会把string强制转换为number类型，如果转换失败则报错
    rule "second-conditions-1"
    when
        $p:Person()
    //    Person(age =="22")
        Person(age ==22)
    then
        System.out.println("second-condition-1执行了"+$p.getName());
    end
    
    4. 逗号, 和&&可以互相代替。但是不可以混合使用。
    // 正确案例
    Person( age > 50, weight > 80 )
    Person( age > 50 && weight > 80 )
    // 错误的案例
    // Do not use the following format:
    Person( ( age > 50, weight > 80 ) || height > 2 )
    
    // Use the following format instead:
    Person( ( age > 50 && weight > 80 ) || height > 2 )
    
    5. 如果要给属性的某个值给赋值变量。采用下面的第二种方式。
    // Do not use the following format:
    Person( $age : age * 2 < 100 )
    
    // Use the following format instead:
    Person( age * 2 < 100, $age : age )
    ```