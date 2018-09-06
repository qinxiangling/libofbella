#Maven

##简介
Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。
Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。由于 Maven 的面向项目的方法，许多 Apache Jakarta 项目发文时使用 Maven。
最初在Jakata Turbine项目中用来简化构建过程。当时有一些项目（有各自Ant build文件），仅有细微的差别，而JAR文件都由CVS来维护。于是希望有一种标准化的方式构建项目，一个清晰的方式定义项目的组成，一个容易的方式发布项目的信息，以及一种简单的方式在多个项目中共享JAR

#Maven的坐标和仓库

**坐标**：groupId,artifactId,version

**构件**：依赖、插件、构建的输出，构件通过坐标作为其唯一标识

**仓库：**

* 远程仓库
* 镜像仓库
* 本地仓库

**远程仓库配置:**

```
apache-maven-3.5.0/lib/maven-model-builder-3.5.0.jar/org/apache/maven/model/pom.xml
```
**镜像仓库配置：**

```
apache-maven-3.5.0/conf/settings.xml
```
**本地仓库：**
>默认.m2目录下,修改默认目录在settings文件中添加如下配置
```
<localrepository>$PATH</localrepository>
```


项目在编译时会通过pom.xml中的依赖配置到本地仓库中查找jar包 找到后添加到项目中,如果本地没有则通过setting.xml文件设置的中央仓库中查找

#Mvane项目编译过程
![Markdown Screenshot](./maven添加jar包过程.jpg)

#常用指令
* mvn package 打包
* mvn compile 编译
* mvn test 运行测试用例
* mvn clean 清除target
* mvn install 安装jar到本地仓库
* mvn archetype:generate 创建一个maven骨架的项目(参数如下)
	* -DgroupId=xxxx
	* -DartifactId=xxx
	* -Dversion=xxx	
	* -Dpackage=xxx

#构建生命周期
| 阶段  | 处理  | 描述 |
|:------:|:-------:|:---:|
| 准备资源 	|  资源复制  	| 资源复制可以进行定制 |
| 编译    	| 	执行编译 	| 源代码编译在此阶段完成 |
| 包装    	| 	 打包   	|创建JAR/WAR包如在 pom.xml 中定义提及的包|
|	安装	|	安装		|这一阶段在本地/远程Maven仓库安装程序包|
**clean清理项目:**

| 		阶段 	|  			描述 	|
|:----------:|:--------------:|
| pre-clean 	| 执行清理前的工作 |
| clean    	| 清理上一次构建生成的所有文件 |
| post-clean	| 执行清理后的文件|
**default构建项目:**
>compile -> test -> package -> install

**site生成项目站点:**

| 		阶段 	|  			描述 				|
|:----------:	|:-----------------------:	|
| pre－site 	| 在生成项目站点前要完成的工作 	|
| site    	| 生成项目的站点文档			|
| post-clean	| 在生成项目站点后要完成的工作	|
|site－ deploy|发布生成的站点到服务器上		|


#maven插件
**maven插件首页:**
```
http://maven.apache.org/plugins/index.html
```

**使用方法**: 转跳 #pom.xml解析#

run build... -> goals:clean package

#pom.xml解析
```
<projectxmins=http:/maven.apache.org/pom/40.0xmin xsischemalocation=httpw/mavenapache.org/pom400...>

	＜!-－指定了当前pom的版本信息-->
	<modelversion>4.0.0</modelversion>
	
	＜grouped＞反写的公司网址＋项目名＜／ grouped＞
	＜artifact＞顶目名＋模块名＜／artifact>
	
	＜!－-第一个0表示大版本号
	9第二个0表示分支版本号
	第三个0表示小版本号
	0.0.1
	snapshot快照
	alpha内部测试
	beta公测
	Releasel稳定
	GAE式发布-->
	<version></version>

	<!-－默认是jar
	other: war zip pom-->
	<packaging></packaging>
	
	<name> 项目描述名 </name>
	<url> 项目地址 </url>
	<description> 项目详情描述 </description>
	<developers> 开发人员列表 </developers>
	<licenses> 许可征 </licenses>
	<organization> 组织 </organization>

<dependencies>
	<dependency>
		<modelversion></modelversion>
		＜grouped＞＜／grouped＞
		＜artifact＞＜／artifact>
		<type></type>
		<scope>test(依赖范围,只在test内生效)</scope>
		<optional>设置依赖是否可选（true,false）<optional>
	
		<!-- 排除依赖传递列表 -->
		<exclusions>
			<exclusion>
			</exclusion>
		</exclusions>
		
	</dependency>
</dependencies>

<dependencyManagenment>
	<!-- 依赖管理，不会添加到项目中，一般用于给继承项目 -->
</dependencyManagenment>

<parent>
	<!-- 所继承的父项目 -->
</parent>

<modules>
	<module>
		<!-- 需要一起编译的其他模块 -->
	</module>
</modules>

<!-- 插件 -->
<build>
	<plugins>
		<plugin>
			<!-- 插件的坐标 -->
			<groupid>org. apache maven plugins</groupid>
			<artlfactid>maven-source-plugin</artifactid>
			<version>2. 4</version>
			<executions>
				<execution>
					<!-- 指定运行的阶段 -->
					<phase> packages </phase>
					<goals>
						<!-- 运行目标 -->
						<goal>jar-no-fork</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>


</project>
```

#maven依赖范围
**官方文档：**
```
http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Scope
```

**三种classpath：**

* 编译
* 测试
* 运行

**scope的属性：**

* compile 默认值，全程有效
* provided 测试和编译有效(junit)
* runtime 测试和运行(例如 jdbc)
* system 和provided一样,但是与本机系统相关联,有不可移植性 
* import 导入的范围,它只使用在dependencyManagement中,表示从其他的pom中导入dependecy的配置

#maven的依赖传递和依赖冲突
**依赖传递:**

A—>B,B->C

则A项目会引入B,C两个jar包.

可用如下方法取消依赖传递：

```
<exclusions>
	<exclusion>
	＜groupId＞C＜／groupId＞
	＜artifactId＞C＜／artifactId>
	</exclusion>	
</exclusions>
```
**依赖冲突:**
>版本产生冲突,则A会引入依赖关联少的Jar包。如下例,A引入的是X_2.0.jar

1.A->B->C->X_1.0.jar

2.A->D->X_2.0.jar
>如果依赖关联长度相同,则引入先声明的版本,如下列,A引入的是X_1.0.jar

1.A->B->X_1.0.jar

2.A->C->X_2.0.jar

```
<dependencies>
	<dependency>
		＜grouped＞B</grouped＞
		＜artifact＞B＜/artifact>
	</dependency>
	<dependency>
		＜grouped＞C＜/grouped＞
		＜artifact＞C＜/artifact>
	</dependency>
</dependencies>
```
#Maven的聚合继承
**聚合:**
install 时会一起构建

```
<modules>
	<module>../A项目</module>
	<module>../B项目</module>
</modules>
```

**继承:**

父类：

```
<properties>
	<b.version>1.2.1</b.version>
<properties>

<dependencyManagenment>
	<dependencies>
	<dependency>
		＜grouped＞B</grouped＞
		＜artifact＞B＜/artifact>
		<version>${b.version}</version>
	</dependency>
</dependencies>
</dependencyManagenment>
```
子类：

```
<parent>
	＜groupId＞parent.groupId＜/groupId＞
	＜artifactId＞parent.artifactId＜/artifactId>
	<version>parent.version </version>
</parent>

<dependencies>
	<dependency>
		＜grouped＞B</grouped＞
		＜artifact＞B＜/artifact>
	</dependency>
</dependencies>
```
