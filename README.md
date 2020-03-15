# 相关文章

# 测试说明
测试未测试全部的请求方式，根据影响的区别挑取两种进行测试：链接跳转和iframe嵌套

### 1. 测试：Chrome默认模式下，对于不使用SameSite设置链接跳转结果

在page1目录下执行：
```
http-server
```
在page2目录下执行：
```
http-server
```
执行后打开控制台，page1使用实际本地IP访问，page2使用127.0.0.1访问

此时访问page2的Chrome控制台中，可以看到cookie仅仅有默认值，Lax和Strict三个

点击page1的链接打开page2，控制台可以看到携带了相关的page2的未设置SameSite属性的Cookie和SameSite=Lax舒心的cookie

查看iframe的请求结果，可以看到携带了page2的未设置SameSite属性的cookie

### 2.测试: 开启Chrome的默认SameSite=Lax
此时执行测试1步骤，会发现iframe将不再包含未设置SameSite属性的cookie

### 3.测试: 设置SameSite=None;Secure
在page2目录下执行：
```
http-server -S -C ../cert.pem -K ../key.pem
```
此时访问page2的Chrome控制台中，可以看到cookie仅仅有默认值，Lax和Strict，None四个值

替换page1中对于page2的引用，点击链接跳转和iframe请求查看，会发现仅仅包含了设置SameSite=None的属性值


# 开启http-server的https
### 1. 生成本地key.pem和cert.pem
``` 
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
```
### 2. 执行指令
```
http-server -S -C cert.pem -K key.pem
```
