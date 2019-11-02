5.4-5.10

主要介绍依赖

5.5依赖范围

![avatar](https://github.com/hideaki10/MavenAction/blob/master/Maven范围依赖.png)

5.6传递性依赖

项目里有Com.icegreen.greenmail的直接依赖，依赖范围是test

com.icegreen.greenmail又依赖javax.mail，javaxmail依赖范围是compile

所以javax.mail是项目的传递依赖，依赖范围是test

单看依赖传递还是很好理解的，但是加入依赖范围就比较容易搞混。所以总结了一张表

![avatar](https://github.com/hideaki10/MavenAction/blob/master/Maven传递依赖.png)





