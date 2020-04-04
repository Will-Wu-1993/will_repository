### Session的原理：
1. Session是保存在服务器段，理论上是没有限制的，只要内存够大。
2. 浏览器第一次访问服务器时会创建一个Session对象，并返回一个JSESSIONID的值，创建一个cookie对象key为JSESSION，value为ID的值，将这个cookie写会浏览器
3. 浏览器在第二次访问服务器的时候携带cookie信息JSESSIONID的值，如果该JSESSIONID的Session已经销毁，那么会重新创建一个新的Session再返回一个新的JSESSIONID通过cookie返回到浏览器。


### Session和cookie的区别：
1. cookie和Session都是会话技术，cookie是运用在客户端，Session是运行在服务器端。
2. cookie有大小限制以及浏览器在存cookie的个数也有限制，Session是没有大小限制的，和服务器内存的大小有关。
3. cookie有安全隐患，通过拦截或本地文件找的到你的cookie后可进行攻击。
4. Session是保存在服务器端上，会存在一段时间才会消失，如果Session过多会增加服务器的压力。