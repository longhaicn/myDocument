# maven介绍与配置

## 1. 介绍

Maven是一个采用java编写的开源项目管理工具，采用一种被称之为Project Object Model（pom）概念来管理项目，所有的项目信息都被定义在一个叫pom.xml的文件中，通过该文件，maven可以管理项目整个生命周期，包括清除、编译、测试、报告、打包、部署等。

## 2. maven能做什么

- jar包的声明式依赖性管理
- 项目生命周期

## 3. pom介绍

`modelVersion`： 当前pom版本号

`groupId`：当前jar命名空间

`artifactId`： 当前模块名称

`version`：当前项目版本  -SNAPSHOT为快照版

`packaging`： jar包 war包

`dependencies`：当前项目依赖的jar包

`properties`：可以配置常量 ${}引用

`parent`:  包含groupId, artifactId, version，继承父pom，子依赖中不用写版本号

## 4. 常用命令

`mvn clean`

`mvn compile`

`mvn test`

`mvn package`

`mvn install`

`mvn deploy`

`mvn  dependency:tree` 打印当前项目的依赖树

## 5. maven核心概念

### 5.1 插件

maven仅仅定义了生命周期，具体任务是交由插件完成的，每个插件都能实现多个功能，每个功能就是一个插件目标，maven插件在`.m2\repository\org\apache\maven\plugins `

### 5.2 坐标

`groupId` `artifactId` `version`唯一定位一个jar包

jar包的路径 `groupId/artifactId/version/artifactId-version `

## 6. 跳过单元测试

```shell
mvn install -D maven.test.skip=true 
```

也可以用surefire插件

```xml
<build> 
    <plugins> 
        <plugin> 
            <artifactId>maven-surefire-plugin</artifactId> 
            <version>2.7.1</version> 
            <configuration> 
            	<skip>true</skip> 
            </configuration> 
        </plugin> 
    </plugins> 
</build>
```

在测试用例前加@Ignore

```java
@Ignore
@Test
public void testGetAreaChirldren() {
        Area area = addArea();
        List<AreaTreeVO> listAreaTreeVOs = areaService.getAreaChirldren(area.getId());
        Assert.assertNotNull("有子节点", listAreaTreeVOs);
}
```

在编写maven构建命令时加上 -Dtest=**，则执行指定的测试用例，*为通配符，例如： 

`clean test -Dtest=*ServiceTest`



## 7. 快速构建项目

```shell
mvn archetype:create -D groupId=com.test.maven -D artifactId=test1 -D packageName=a.b.c 
```

archetype可以快速构建项目框架

## 8. version版本号介绍

version：版本号，比如：0.0.1-SNAPSHOT

版本号的定义为：x.x.x-里程碑，第一个x表示大版本（大调整，架构上的变化），第二个x表示分支（大版本下的分支），第三个x表示分支里做了多少次更新；

里程碑有SNAPSHOT(正在开发中的版本)，alpha(开发完后，内部的测试版本)，beta(项目测试完没问题后定义该版本，可供使用人员下载下来用)，用了一段时间后没问题了就定义为Release(RC)版本，最后会生成一个可靠版本GA。