# 路径匹配问题

## 问题描述

>在开发过程中，我们通常在拦截器中解决解决一些通用的问题，比如基于url进行权限校验，常用的方式就是判断当前的是否与指定的相等，
随着业务的增加，拦截的就会越来越多，那么如何优雅的进行拦截呢？

## 解决方案

### 方案一: 利用正则表达式
采用正则表达式进行模糊匹配，优点是十分灵活，想怎么玩，就怎么玩。缺点就是由于太灵活了，导致无法全部穷举，并且需要自己去封装相应的规则。

### 方案二: 利用spring中工具类
`AntPathMatcher`就是一个工具辅助类，帮助我们匹配路径是满足，参考 apache 的 ant项目的路径匹配规则，
说到这里，那么就必须先了解一下，apache旗下的`Ant`匹配规则如下，

网上的不够权威，大部分都是千篇一律，这里笔者特意去官方网站去查询了一下，参考于[Ant-style](http://ant.apache.org/manual/dirtasks.html) 路径匹配模式语法

| Example | Explanation |
| --- | --- |
|  \*\*/CVS/\* | Matches all files in directories that can be located anywhere in the directory tree. Matches:  But not: ( part does not match) CVS CVS/Repository; org/apache/CVS/Entries; org/apache/jakarta/tools/ant/CVS/Entries; org/apache/CVS/foo/bar/Entries foo/bar/.  |
| org/apache/jakarta/** | Matches all files in the directory tree.Matches: But not: ( part is missing).org/apache/jakarta;org/apache/jakarta/tools/ant/docs/index.html;org/apache/jakarta/test.xml;org/apache/xyz.java;jakarta/ |
| org/apache/\*\*/CVS/\* | Matches all files in directories that are located anywhere in the directory tree under .Matches: But not: ( part does not match)CVSorg/apache;org/apache/CVS/Entries;org/apache/jakarta/tools/ant/CVS/Entries;org/apache/CVS/foo/bar/Entries;foo/bar/. |
| \*\*/test/\*\* | Matches all files that have a element in their path, including as a filename.testtest |

但是，spring的`AntPathMatcher`比这个更加的强大

这里笔者就不贴`AntPathMatcherTests`源码的，汇总成以及几个表格。

#### match、matchStart方法
主要用于校验是否匹配成功

##### 测试精确匹配

| 表达式 | 匹配的路径 | 执行结果（boolean） |
| --- | --- | --- |
| `test` | test | true |
| `/test` | test | **false** |
| `test` | /test | **false** |

---
##### 测试通配符 `?` （只能匹配一个字符）

| 表达式 | 匹配的路径 | 执行结果（boolean） |
| --- | --- | --- |
| `t?st` | test | true |
| `t???st` | tedfst | true |
| `tes?` | test | true |
| `tes?` | testt | **false** |
| `?es?` | test | true |

---
##### 测试通配符 `*` （匹配n字符）

| 表达式 | 匹配的路径 | 执行结果（boolean） |
| --- | --- | --- |
| `*` | test | true |
| `test*` | test | true |
| `test*` | testTest | true |
| `test*` | test/ | **false** |
| `test/*` | test/Test | true |
| `test/*` | test/ | true |
| `test/*` | test | **false** |
| `*test*` | AnothertestTabc | true |
| `*test` | Anothertest | true |
| `*.*` | test. | true |
| `*.*` | test.test | true |
| `test*aaa` | testblaaaa | true |

---
##### 测试通配符 `**` （匹配n字符）

| 表达式 | 匹配的路径 | 执行结果（boolean） |
| --- | --- | --- |
| `/**` | /testing/testing | true |
| `/*/**` | /testing/testing | true |
| `/**/*` | /testing/testing | true |
| `/bla/**/bla` | /bla/testing/testing/bla/bla | true |
| `/bla*bla/test` | /blaXXXbla/test | true |
| `/*bla*/**/bla/**` | /XXXblaXXXX/testing/testing/bla/testing/testing.jpg | true |

---
#### extractPathWithinPattern方法
返回模糊匹配的部分

| 表达式 | 匹配的路径 | 执行结果（String） |
| --- | --- | --- |
| `/docs/*` | /docs/cvs/commit | cvs/commit |
| `/d?cs/**/*.html` | /docs/cvs/commit.html | docs/cvs/commit.html |

---
#### extractUriTemplateVariables方法
主要用于获取路径`{var}`中的值

| 表达式 | 匹配的路径 | 执行结果（map） |
| --- | --- | --- |
| `/hotels/{hotel}` | /hotels/1 | {"hotel":"1"} |
| `/{name}.{extension}` | /test.html | {"name":"test","extension":"html"} |
| `/A-{B}-C` | /A-b-C | {"B":"b"} |
