# Drools文件

## 1、结构体

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

