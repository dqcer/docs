
# mermaid
> ```mermaid
>  graph TD; 
>  A-->B;
>  A-->C;  
>  B-->D;
>  C-->D;
>  ```

### Section X
```plantuml
@startuml
hide empty description
left to right direction

[*]-->NotLogin:首次打开app

state  NotLogin: 没有登录
state  LoginSuccess: 登录成功
state  LoginFail: 登录失败

NotLogin -->LoginSuccess: 登录
NotLogin -->LoginFail:登录

state LoginFail {
    state 用户不存在: ta校验
    state saas注册失败:注册saas
    state 登记用户信息失败: 登记到tiananUser
    state 参数校验失败 :校验参数
}

state join_state  <<join>>
LoginSuccess-->join_state:组装响应参数
LoginFail-->join_state:组装响应参数

join_state -->[*]:返回JSON数据到客户端

@enduml
```