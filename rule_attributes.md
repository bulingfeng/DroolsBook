### 1、no-lop
保证规则规则值执行一次。如果此时有人改变了fact的值，并且使用update,insert等操作可能出现死循环。
```drl
rule "noloopTest-1"
no-loop true
when
    $noLoop:NoLoopEntity(isExe==true)
then
    System.out.println("no-loop-1 执行了");
    update($noLoop)
end
 //下面会重复执行造成死循环
rule "noloopTest-2"
no-loop true
when
    $noLoop:NoLoopEntity(isExe==true)
then
    System.out.println("no-loop-2 执行了");
    update($noLoop)
end
```
所以no-loop并不是一个严格意义上的只执行一次。严格执行一次的关键字为lock-on-active

### 2、salience
定义规则的执行顺序的优先级。数字越大说明越先执行。默认值为1.Example: salience 10

### 3、enabled
当enabled为true时候，当规则满足条件会执行。放enabled为false则反之。
```drl
rule "enabledTest-rule-1"
    enabled true
when
    Message(message=="enabled")
then
    System.out.println("enabled-rule-1 is running");
end
// ------ 不会执行 enabledTest-rule-2
rule "enabledTest-rule-2"
    enabled false
when
    Message(message=="enabled")
then
    System.out.println("enabled-rule-2 is running");
end
```
### 4、date-effective
时间的有效期开始时间。就是规则从这个时间点开始执行。这个时间点之前的规则不会执行。
Example: date-effective "4-Sep-2018"

### 5、date-expires
时间的过期时间，在这个是规则执行的最后日期。超越这个时间规则即使满足条件也不会执行。
Example: date-expires "4-Oct-2018"

### 6、agenda-group
当指定agenda-group后,会匹配所配置的agenda-group名称。下面的代码只会执行agenda-group为
agendaGroup-rule-1内的规则
```java
KieServices ks = KieServices.Factory.get();
        KieContainer kContainer = ks.getKieClasspathContainer();
        KieSession kSession = kContainer.newKieSession("rulesSession");
        kSession.getAgenda().getAgendaGroup("agendaGroup-1").setFocus();
        Message message=new Message();
        message.setMessage("AgendaGroupRule");
        kSession.insert(message);
        int count=kSession.fireAllRules();
        System.out.println("enabled 一共执行了"+count+"条规则");
        kSession.dispose();
```
```drl
rule "agendaGroup-rule-1"
    agenda-group "agendaGroup-1"
when
    Message(message=="AgendaGroupRule")
then
    System.out.println("AgendaGroupRule-1 is running");
end

rule "agendaGroup-rule-2"
    agenda-group "agendaGroup-1"
when
    Message(message=="AgendaGroupRule")
then
    System.out.println("AgendaGroupRule-2 is running");
end

rule "agendaGroup-rule-3"
    agenda-group "agendaGroup-2"
when
    Message(message=="AgendaGroupRule")
then
    System.out.println("AgendaGroupRule-3 is running");
end
```

### 7、activation-group
该规则和agenda-group类似。activation-group内最多会执行一条规则。而agenda-group则是只要有匹配
都会执行。
