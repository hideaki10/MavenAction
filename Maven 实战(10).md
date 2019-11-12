7.1 ～ 7.2.4 生命周期

生命周期听着挺抽象的，其实就是指项目的构建步骤。Maven有3套生命周期。

1. clean生命周期 → 项目的清理
2. default生命周期 → 项目的构建
3. sit生命周期 → 项目的建立和发布

Maven用3个生命周期来定义了项目从清理到分布的步骤。每个生命周期内分为各种阶段。

但是生命周期只是个抽象的概念，具体的实现就要用到Maven的另一个核心东西-插件。

7.2.5 命令行和生命周期

Maven利用命令行来指定生命周期的开始阶段和结束阶段。

比如 : mvn clean就指定 从clean生命周期的 pre-clean阶段开始到 clean阶段结束

7.3 插件目标

前面说到具体实现生命周期里的操作的是插件。一个插件里会有很多功能，而每一个功能就是

一个插件。

比如 :  maven-dependency-plugin这个插件有十多个目标，也就是说十多个功能。我们经常用到

1.dependency:analyze

2.dependency:list

3.dependency:tree

dependency是插件的名字， analyze.list,tree是插件目标 

