6.1 Maven仓库是什么

在Maven之前，一般我们在项目里吧jar包复制到lib目录下来管理。如果在A项目里面使用log，

我们讲log4j的jar导到A项目里，又在B项目里使用log，我们又要讲log4j导到B项目里。然后又有C

项目，D项目。我们要一个个到进去。非常的麻烦的不说，万一当中漏了一个，项目编译又要报错。

所以Maven使用仓库来解决这个问题。吧jar包都当放到仓库里，那个项目要用jar包就去仓库里取。

那怎么取了。就是用上一章介绍过的坐标系统来确定jar包的位置。

6.2 仓库的布局

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



