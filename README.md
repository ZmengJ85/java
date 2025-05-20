一、安装和配置JDK
①进入/usr/lib/jvm目录，然后安装JDK
[root@host1 ~]# cd /usr/lib/jvm
[root@host1 jvm]# yum install  -y java-1.8.0-openjdk-devel

②配置环境变量，编辑/etc/profile文件，在其末尾加上以下语句并保存该文件。
[root@host1 jvm]# vi /etc/profile
export Java_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export JRE_HOME=$Java_HOME/jre
export CLASSPATH=$Java_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$Java_HOME/bin:$JRE_HOME/bin:$PATH

③使环境变量生效。
[root@host1 jvm]# source /etc/profile

二、安装Maven软件
①　从Maven官网下载其二进制安装包，下载的是apache-maven-3.9.9-bin.tar.gz。
Download Apache Maven – Maven
Wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz

②　将该软件复制到/usr/local目录，切换到该目录，执行以下命令。
[root@host1 local]# tar zxvf apache-maven-3.9.9-bin.tar.gz

③　编辑/etc/profile文件，在其末尾加上以下语句并保存该文件。
[root@host1 local]# vi /etc/profile
export PATH=/usr/local/apache-maven-3.9.9/bin:$PATH

④　执行source /etc/profile命令使环境变量生效。
[root@host1 local]# Source /etc/profile

⑤　测试Maven是否已正确安装
[root@host1 local]# Mvn -v。

三、创建一个简单的Java应用程序
①  在ch06下新建目录：hello-world，进入项目目录，执行以下命令创建一个Java项目。
[root@host1 local]# cd 
[root@host1 ~]# mkdir ch01
[root@host1 ~]# cd ch01/
[root@host1 ch01]# mkdir hello-world
[root@host1 ch01]# cd hello-world/
[root@host1 hello-world]# mvn archetype:generate -DgroupId=org.examples.java -DartifactId=helloworld 

② 执行以下操作构建项目。
[root@host1 hello-world]# cd helloworld
[root@host1 helloworld]# mvn package

③ 运行所生成的Java类。
[root@host1 helloworld]# java -cp target/helloworld-1.0-SNAPSHOT.jar org.examples.java.App


四、下载和运行Java镜像
以交互方式运行OpenJDK容器，在容器中打开一个终端，在其中执行java –version命查看Java版本。
[root@host1 helloworld]# docker container run -it openjdk:8
[root@host1 helloworld]#Java -version


五、将Java应用程序打包为镜像并启动容器运行该程序
① 在项目目录中创建一个Dockerfile文件，添加以下内容并保存该文件。
[root@host1 hello-world]# vi DockerfileFROM openjdk:8
FROM openjdk:8

COPY target/helloworld-1.0-SNAPSHOT.jar /usr/src/helloworld-1.0-SNAPSHOT.jar
CMD java -cp /usr/src/helloworld-1.0-SNAPSHOT.jar org.examples.java.App

② 基于该Dockerfile文件构建镜像。
[root@host1 hello-world]#  docker image build -t hello-java:latest .

③ 运行此镜像并启动容器。
进入helloworld目录，执行以下命令：
[root@host1 hello-world]# cd helloworld/
[root@host1 helloworld]# docker run --rm hello-java:latest
