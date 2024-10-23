# koupleless-adapter-tongweb
组件是作为 [Koupleless](https://github.com/koupleless/koupleless) 项目的一个扩展组件，目的是适配东方通（TongWEB)容器。

由于TongWEB是商业软件，目前维护在个人仓库中。

项目目前仅在tongweb-embed-7.0.E.6_P7 版本中验证过，其他版本需要自行验证，必要的话需要根据相同的思路进行调整。

如果不需要使用同一端口来安装BIZ模块,只需要关注下文安装依赖章节提到的注意事项即可，不需要引入本项目相关的依赖。

## 快速开始
### 1. 安装依赖
首先需要确保已经在maven仓库中导入了TongWEB相关的依赖。
(这里有个关键点，由于koupleless 2.2.14项目对于依赖包的识别机制与TongWEB的包结构冲突，需要将TongWEB的依赖包加上sofa-ark-的前缀,具体的识别机制可参考koupleless的com.alipay.sofa.ark.container.model.BizModel类)

增加的install脚本参考如下：
```angular2html
@echo off

setlocal

set "CURRENT_DIR=%cd%"
if not "%EMBED_HOME%" == "" goto gotHome
set "EMBED_HOME=%CURRENT_DIR%"

:gotHome
if exist "%EMBED_HOME%\install-sofa-ark-tongweb.bat" goto OK_HOME
echo current dir [%EMBED_HOME%] is not correct.
echo Please enter the directory containing the specified command [installMavenJar.bat].
goto end

:OK_HOME

call mvn install:install-file -DgroupId=com.tongweb.springboot -DartifactId=sofa-ark-tongweb-spring-boot-starter -Dversion=7.0.E.6_P7 -Dfile="%EMBED_HOME%/lib/sofa-ark-tongweb-spring-boot-starter-7.0.E.6_P7.jar" -Dpackaging=jar
call mvn install:install-file -DgroupId=com.tongweb -DartifactId=sofa-ark-tongweb-embed-core -Dversion=7.0.E.6_P7 -Dfile="%EMBED_HOME%/lib/sofa-ark-tongweb-embed-core-7.0.E.6_P7.jar" -Dpackaging=jar
call mvn install:install-file -DgroupId=com.tongweb -DartifactId=sofa-ark-tongweb-lic-sdk -Dversion=4.5.0.0 -Dfile="%EMBED_HOME%/lib/sofa-ark-tongweb-lic-sdk-4.5.0.0.jar" -Dpackaging=jar



pause
:end
```
### 2. 编译安装本项目插件
进入本项目的tongweb7-web-adapter 目录执行mvn install命令即可。
项目将会安装“tongweb7-web-ark-plugin” 和 “tongweb7-sofa-ark-springboot-starter” 两个模块。

### 3. 使用本项目组件
首先需要根据koupleless的文档，[将项目升级为Koupleless基座](https://koupleless.io/docs/tutorials/base-create/springboot-and-sofaboot/)

然后将依赖中提到的
```
    <dependency>
        <groupId>com.alipay.sofa</groupId>
        <artifactId>web-ark-plugin</artifactId>
        <version>${sofa.ark.version}</version>
    </dependency>
```
替换为本项目的坐标
```
    <dependency>
        <groupId>com.alipay.sofa</groupId>
        <artifactId>tongweb7-web-ark-plugin</artifactId>
        <version>${sofa.ark.version}</version>
    </dependency>
    
    <dependency>
        <groupId>com.alipay.sofa</groupId>
        <artifactId>tongweb7-sofa-ark-springboot-starter</artifactId>
        <version>${sofa.ark.version}</version>
    </dependency>
```

引入TongWEB相关依赖（同时需要exclude tomcat的依赖）。参考依赖如下：
```angular2html
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>com.tongweb.springboot</groupId>
            <artifactId>sofa-ark-tongweb-spring-boot-starter</artifactId>
            <version>7.0.E.6_P7</version>
        </dependency>
        <dependency>
            <groupId>com.tongweb</groupId>
            <artifactId>sofa-ark-tongweb-embed-core</artifactId>
            <version>7.0.E.6_P7</version>
        </dependency>
        <dependency>
            <groupId>com.tongweb</groupId>
            <artifactId>sofa-ark-tongweb-lic-sdk</artifactId>
            <version>4.5.0.0</version>
        </dependency>
```

### 4. 完成
完成上述步骤后，即可在Koupleless中使用TongWEB启动项目。
