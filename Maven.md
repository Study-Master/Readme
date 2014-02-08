Maven简单的说明
===

Ciel大仙

##简介
Maven是Java的构建工具。一般来说，我们的程序会使用大量的外部库，这一类库往往都是Jar文件，在编译时，需要手动指定引用。Maven简化了这一类过程，它可以方便的管理Java程序中的依赖。并且快速构建Java目标文件，并完成Jar打包。Maven可以完美的和eclipse兼容，可以将项目快速转变为eclipse项目，也可以将编写完的项目，清除eclipse配置，成为发布用的IDE－Free项目。Maven是一个命令行工具，因此需要简单的命令行操作。

##确认Maven已经安装
打开Terminal（Windows打开cmd命令提示符），输入`mvn -v`或者`mvn --version`。如果一切正常，你将会看到类似

```Bash
Apache Maven 3.1.1 (0728685237757ffbf44136acec0402957f723d9a; 2013-09-17 23:22:22+0800)
Maven home: /usr/local/apache-maven-3.1.1
Java version: 1.7.0_51, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.7.0_51.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.9.1", arch: "x86_64", family: "mac"
```

的输出结果。如果不是，请登陆[Maven官网](http://maven.apache.org)，下载最新版本的Maven，解压后根据README文件中提示进行操作。

##利用Maven启动一个新项目
将Terminal `cd`到你需要创建项目的文件夹，Maven将会在该文件夹创建一个新的项目文件夹。

>> 如果没有使用过Terminal，这里有几条基本命令。
>> 
>> 1. `ls`命令，该命令用于列出当前文件夹下的所有文件。Windows等价于`dir`命令。
>> 2. `cd`命令，后面跟随路径名，即切换文件夹。如`cd Documents/JavaProject`，可以直接拷贝路径名到后面。

此时，在Terminal下输入

```Bash
mvn archetype:generate
```

Maven会出现交互界面，需要输入一些我们要开发的应用的信息。

* Choose a number: 回车即可，不需要特殊操作
* Define value for groupId: 输入组织id，比如org.studymaster
* Define value for artifactId：输入项目名称，比如Http-Client
* Define value for version: 输入版本号，可以直接回车，默认是1.0-SNAPSHOT
* Define value for package: java的包名，比如org.studymaster.httpclient

输入完毕后回车确认，Maven就会创建一个用项目名命名的项目文件夹，本例中为Http-Client文件夹。使用`cd Http-Client`切换到该文件夹下，便可开始工作。

##Maven的项目Layout
可以看到，Maven在该项目下创建了一个`pom.xml`文件和一个`src`文件夹。

* `pom.xml`文件将储存项目中需要的依赖的配置和项目配置。
* `src`文件夹下储存了两个子文件夹，一个代码文件夹，另一个为测试代码文件夹。默认情况下，Maven自动导入`JUnit`库做Unit Test。

##开始第一个HelloWorld程序
进入到`src`目录下的`main`的子目录中，发现Maven已经根据package自动设定好了文件夹。在最深目录中会有`App.java`文件。该文件已经声明好了主类，并且生命好了package和写了一行HelloWorld代码。我们现在开始编译这个项目，并输出HelloWorld。

用Terminal cd到Http-Client文件夹下，输入

```Bash
mvn package
```

Maven便开始编译整个项目。期间会执行如下几个操作：

* 根据pom.xml文件中的声明下载需要的类库。
* 创建`target`目录。
* 编译代码。
* 编译测试代码。
* 使用JUnit测试并生成测试报告。
* 打包编译完毕的文件，生成打包完毕的jar文件。

编译打包过程中会出现类似的提示：

```Bash
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building Http-Client 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ Http-Client ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/Ciel/Documents/Http-Client/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:2.5.1:compile (default-compile) @ Http-Client ---
[INFO] Compiling 1 source file to /Users/Ciel/Documents/Http-Client/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ Http-Client ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/Ciel/Documents/Http-Client/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:2.5.1:testCompile (default-testCompile) @ Http-Client ---
[INFO] Compiling 1 source file to /Users/Ciel/Documents/Http-Client/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ Http-Client ---
[INFO] Surefire report directory: /Users/Ciel/Documents/Http-Client/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running org.studymaster.httpclient.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.005 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ Http-Client ---
[INFO] Building jar: /Users/Ciel/Documents/Http-Client/target/Http-Client-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.890s
[INFO] Finished at: Sat Feb 08 22:40:32 SGT 2014
[INFO] Final Memory: 13M/156M
[INFO] ------------------------------------------------------------------------
```

其中`[INFO] Building jar: /Users/Ciel/Documents/Http-Client/target/Http-Client-1.0-SNAPSHOT.jar`说明了生成了编译打包完毕的Jar文件。现在，我们可以执行打包完成的Jar文件。

在Terminal中输入：

```Bash
java -cp target/Http-Client-1.0-SNAPSHOT.jar org.studymaster.httpclient.App
```
其中`-cp`后面的第一个参数表示Jar文件路径，第二个参数`org.studymaster.httpclient.App`指出主类（因为我们的package名称是`org.studymaster.httpclient`，main method在`App.java` 中，所以主类是App）。之所以需要输入这么多是因为Maven在编译打包过程中并没有被指定主类，因此其并没有设定入口文件。下文将有更多介绍。

##Maven常用命令

* 编译源程序：`mvn compile`
* 编译并测试：`mvn test`
* 清空生成的文件：`mvn clean`