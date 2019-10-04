# Drools文件

## 1、结构体
也可以直接参考官方文档 [官方文档](https://docs.jboss.org/drools/release/7.27.0.Final/drools-docs/html_single/index.html#drl-packages-con_drl-rules)
下面图是drl文件结构图,应该按照这种方式来编写

![](/Users/bulingfeng/personal/Book/DroolsBook/Image/1.png)

举例说明:

```drl
package

import

function  // Optional

query  // Optional

declare   // Optional

global   // Optional

rule "rule name"
    // Attributes
    when
        // Conditions
    then
        // Actions
end
```

