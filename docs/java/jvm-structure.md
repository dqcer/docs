

# JVM 内存结构

![jvm-memory-structure](https://cdn.jsdelivr.net/gh/doocs/jvm@main/images/jvm-memory-structure.jpg)

### Section X
```plantuml

@startuml

state  JVM : JVM 内存结构


state JVM {
    state demo: ta校验
    state saas注册失败:注册saas
    state 登记用户信息失败: 登记到tiananUser
    state 参数校验失败 :校验参数
    state demo : {
        state 用户不存在1: ta校验1
    }
}


@enduml
```