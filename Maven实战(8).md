5.4-5.10

主要介绍依赖

5.5依赖范围

![avatar](https://github.com/hideaki10/MavenAction/blob/master/Maven范围依赖.png)

5.6传递性依赖

A → B (test) → C (compile)  根据规则来看 依赖范围是test

单看依赖传递还是很好理解的，但是加入依赖范围就比较容易搞混。所以总结了一张表

![avatar](https://github.com/hideaki10/MavenAction/blob/master/Maven传递依赖.png)

5.7 ~ 5..8 依赖调解和可选依赖

依赖调教时候会碰到重复的问题。Maven利用依赖调解来解决这个问题。

依赖调解的2大原则

1. 路径最优者优先

   ①   A → B → C（1.0）

   ②   A → B → D → C （1.1） 

   2个A项目都依赖了C 但是① 的路径是2，② 的路径的3，所以优先选择①  

2. 第一声明者优先

    ①   A → B → C（1.0）

    ②   A → D → C（1.1）

   2个A项目都依赖了C ,而且2个路径都是2，所以就用到第2个原则，谁先声明就选择这个依赖

可选依赖就是依赖不会传递，一般只想在当前项目下使用，不想传递别的项目的时候使用。

 5.9.1 ~ 5.9.2 排除依赖和归类依赖

排除依赖就是依赖传递的时候传传进来一个jar包不想在当前项目里使用，需要将它排除出去。

使用了<exclusions>的标签

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.juvenxu.mvnbook</groupId>
  <artifactId>project-a</artifactId>
  <version>1.0.0</version>
  <dependecies>
    <dependency>
      <groupId>com.juvenxu.mvnbook</groupId>
      <artifactId>project-b</artifactId>
      <version>1.0.0</version>
      <exclusions>
        <exclusion>
            <groupId>com.juvenxu.mvnbook</groupId>
            <artifactId>project-c</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>com.juvenxu.mvnbook</groupId>
      <artifactId>project-c</artifactId>
      <version>1.1.0</version>
    </dependency>
    </dependencies>
</project>
```



归类依赖就是使用变量来定义相同的设置，和java全局变量的概念一样。

5.9.3 优化依赖

查看依赖的3个命令

1. mvn dependency:list  查看当前项目的以解析的依赖

2. mvn dependency;tree 顾名思义 查看树形状的依赖

3. mvn dependency:analyze 依赖性的分析 

   3.1 used undeclared dependencies 项目中使用到，但没有直接依赖，而是转递依赖。

   这里就要注意，直接依赖升级版本的时候，传递依赖也会跟着升级。只有可能导致版本不一而报错。

   所以如果项目中使用的话，尽量使用直接依赖。

   3.2 unused declared dependencies 项目没有使用到，但是确是直接依赖

   没有使用到，不一定代表没有用。有可能在运行的时候才需要。analyze只会分析编译和测试需要用到的依赖。所以显示出来依赖要不要删除，还是需要确认一下。