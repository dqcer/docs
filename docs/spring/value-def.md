- String 映射
```java

    /**
     *  默认：频繁登录
     */
    @Value("${auth.warn.email.subject:频繁登录}")
    private String subject;
```

- Map 映射

```java

    /**
     *  默认：{1:'小婷',2:'小勤'}
     */
	@Value("#{${site.active.record:{1:'小婷',2:'小勤'}}}")
	private Map<Integer, String> activeMessage;
```

- `${}` 与 `#{}`
    - `#{}` 主要用于加载外部属性文件中的值
    - `${}` 用于执行`SpEl`表达式，并将内容赋值给属性
    - `#{}` 和 `${}` 可以混合使用，但是必须`#{}`外面，`${}`在里面