### 6.1 Maven仓库是什么

在Maven之前，一般我们在项目里吧jar包复制到lib目录下来管理。如果在A项目里面使用log，

我们讲log4j的jar导到A项目里，又在B项目里使用log，我们又要讲log4j导到B项目里。然后又有C

项目，D项目。我们要一个个到进去。非常的麻烦的不说，万一当中漏了一个，项目编译又要报错。

所以Maven使用仓库来解决这个问题。吧jar包都当放到仓库里，那个项目要用jar包就去仓库里取。

那怎么取了。就是用上一章介绍过的坐标系统来确定jar包的位置。

### 6.2 仓库的布局

用了一段Maven的源码来说明，来说明如何根据坐标系统来取得jar文件的。

```java
private static final char PATH_SEPARATOR = '/';
private static final char GROUP_SEPARATOR = '.';
private static final char ARTIFACT_SEPARATOR = '-';

public String pathOf(Artifact artifact){
  ArtifactHandler artifactHandler = artifact.getArtifactHandler();
  StringBuilder path = new StringBuilder(128);
  path.append(formatAsDirectory(artifact.getGroupId())).append(PATH_SEPARATOR);
  path.append(artifact.getArtifactId()).append(PATH_SEPARATOR);
  path.append(artifact.getBaseVersion()).append(PATH_SEPARATOR);
  path.append(artifact.getArtifactId()).append(ARTIFACT_SEPARATOR).
  append(artifact.getBaseVersion());
  if(artifact.hasClassifier()){
    path.append(ARTIFACT_SEPARATOR).append(artifact.getClassifier());
  }
  if(artifactHandler.getExtension() != null && artifactHandler.getExtension().length() > 0)
  {
    path.append(GROUP_SEPARATOR).append(artifactHandler.getExtension());
  }
  return path.toString();
}

private String formatAsDirectory(String directory){
  return directory.replace(GROUP_SEPARATOR,PATH_SEPARATOR);
}

```



### 6.3 仓库的分类

Maven的仓库分为本地和远程，远程又分为中央，私服，公共库。

#### 6.3.1 本地仓库

本地的仓库的默认目录

1. Windows → C:￥Use￥用户名¥.m2¥repository

2. Linux → /home/用户名/.m2/repository

3. Mac → /Users/用户名/.m2/repository   

当然不想用默认目录，自己也可以自定义。自定义有2种方法

1. 修改 ～/.m2/setting,xml 只对当前用户有效

2. 修改 $M2_HOME/conf/setting.xml 对所有用户有效

#### 6.3.2 ～ 6.3.4 远程仓库

Mave的默认远程仓库是中央仓库,可以在超级pom($M2_HOME/lib/maven-x.x.x-uber.jar)里面找到中央仓库

的配置。本地仓库本来是空的，在执行第一个Maven命令的时候，会从中央仓库下载jar包到中央仓库。

还有一种远程仓库是私服，一般在局域网里面使用。从中央仓库哪里下载jar包放倒私服，然后供局域网

里面的Maven用户使用。用私服有几个好处

1. 节省外网带宽

2. 能部署中央仓库没有的第三方的jar包

3. 连接局域网可以增强稳定性

4. 降低中央仓库的负荷

### 6.4 远程仓库的配置

远程仓库除了中央仓库，私服还有第三方的公开仓库。有时候要用到第三方公开仓库，需要在pom里面

配置。

```xml
<project>
  <repositories>
    <id>jboss</id>
    <name>JBoss Repository</name>
    <url>http://reposity.jboss.com/maven2</url>
    <release>
    <enable>true</enable>
    </release>
    <snapshots>
    <enable>false</enable>
    </snapshots>
    <layout>default</layout>
  	</repositories>
</project>
```

配置里面的id 必须是唯一，中央仓库的id是central 如果在pom重新声明了central，那么就会覆盖了默认的中央仓库。

layout的default表示仓库是Maven2和3的布局，而不是Maven1的布局。releases 和 snapshots 的 true和false 来表示是否

下载发布版和快照版本，除了enable的标签还有另外2个标签，updatePolicy和checkumPoily。updatePolicy是指从远程

仓库检查更新的频率。checksumPolicy是用来配置Maven的检验和文件的策略。当jar包被部署到Maven远程仓库时会一

同部署对应的校验和文件。在下载jar包会验证校验和文件，当校验失败时候，根据设定值做出相应的对策。

当warn的时候，会输出警告信息。当fail的时候，会build失败。当ignore的时候，会忽视校验和错误。

#### 6.4.1 远程仓库的认证

一般的远程仓库是不需要认证的。但是有时候在局域网里设置了一个私服的Maven服务器，为每个项目都设置了自己的

仓库，为了防止非法的访问，会给每个仓库配置用户名和密码。同时为了使用仓库，要在本机设置用户名和密码。设置是

在settings.xml里配置，而不是pom里。

```xml
<settings>
	<servers>
    <server>
      <id>my-proj</id>
      <username>repo-user</username>
      <password>repo-pwd</password>
    </server>
  </servers>
</settings>
```

id 的设置必须和 pom里面的 id一致。

#### 6.4.2 部署至远程仓库

自己的项目结束后，需要上传到私服，让别的项目来使用，需要在pom里配置。

```xml
<project>
  <distributionManagement>
  	<resposrity>
    	<id>proj-releases</id>
      <name>Proj Release Repository</name>
      <url>http://xxxxxx</url>
    </resposrity>
    <snapshotRepository>
      <id>proj-snapshots</id>
      <name>Proj Snapshot Repository</name>
      <url>http://xxxxx</url>
    </snapshotRepository>
  </distributionManagement>
  
</project>
```





