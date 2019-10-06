

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