# 一、传统项目开发存在的问题

> 在项目中导入jar包
> 
> > 下载jar包（mvn）
> 
> > 将下载的jar包复制到项目中（WEB-INF/lib）
> 
> > 选择jar文件--右键--Add as Library
> 
> 这种导包方式存在的问题有如下几点。
> 
> > 步骤多（相对而言）--繁琐
> 
> > 在不同项目中若需使用相同的jar包，需分别存储这个jar包（即相同jar包存储多份）--冗余，项目体积大
> 
> > 同一项目在不同环境下运行可能需要专门为其配置依赖环境（jar包版本不一致等）--移植性差

> 无法自定义管理 “项目构建” 的生命周期：
> 
> > IDEA开发环境提供了一键构建项目的功能，但是如果我们需要自定义的项目构建生命周期管理，却没有现成的工具。

---

> jar包与war包的区别
> 
> > jar 是类的归档文件，不仅用于压缩和发布，而且还用于部署和封装库、组件和插件程序，并可被像编译器和 JVM 这样的工具直接使用。
> > 
> > jar 中包含特殊的文件，如 manifests 和部署描述符，用来指示工具如何处理特定的 jar。
> > 
> > 通常是开发时要引用通用类，打成 jar 包便于存放管理，当你使用某些功能时就需要这些 jar 包的支持，需要导入jar包。
> > 
> > jar 包就是 java 的类进行编译生成的 class 文件打包的压缩包，包里面就是一些 class 文件。
> 
> > war 是一个 Web 程序进行打包后生成的便于部署的压缩包，里面包含我们 Web 程序需要的一些东西，其中包括 web.xml 的配置文件，前端的页面文件，以及依赖的 jar。便于我们部署工程，直接放到 tomcat 的 webapps 目录下，直接启动 tomcat 即可。

# 二、Maven简介

Maven 是一个 “基于项目对象模型（POM）的用于进行项目的依赖管理和生命周期管理” 的工具软件。

---

Maven 核心功能

1. 项目的依赖管理

2. 项目的生命周期管理

3. 聚合工程

---

Maven 目录结构

1. bin：存放指令文件（eg：mvn）

2. boot：包含了一个类加载框架的 jar 文件（eg：plexus-classworlds-2.6.0.jar）

3. conf：包含了 Maven 的核心配置文件 settings.xml

4. lib：存放了 Maven 运行所需的 jar 文件

Maven 是基于 Java 开发的工具，因此依赖 jdk。

# 三、Maven安装及配置

> 解压Maven压缩包
> 
> 配置环境变量
> 
> > MAVEN_HOME=C:\Users\REWOPTEG\apache-maven-3.9.0
> 
> > Path=%MAVEN_HOME%\bin
> 
> | 测试                                        |
> |:-----------------------------------------:|
> | ![](images/2023-03-20-17-36-15-image.png) |

# 四、Maven项目结构

> 项目名称
> 
> > src（存放源文件）
> > 
> > > main（存放项目源文件）
> > > 
> > > > java（存放 java 代码，相当于传统项目的 src 目录）
> > > 
> > > > resources（存放静态资源和配置文件，相当于传统项目的 web 目录）
> > 
> > > test（存放项目的单元测试文件）
> > > 
> > > > java（存放单元测试代码）
> 
> > pom.xml（项目对象模型）

# 五、Maven依赖管理

**pom.xml**：依赖配置文件，可以对此项目进行依赖管理。

Maven 可以根据 pom 文件的配置对此项目进行依赖管理。项目中如果需要依赖，可以直接在 pom.xml 进行配置。

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!--project后面添加的是pom.xml文件的语法规则，如果文件中标签不遵循该规则，会报错-->
<project xmlns = "http://maven.apache.org/POM/4.0.0"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!--指定项目模型版本-->
    <modelVersion>4.0</modelVersion>

    <!--指定项目的标识，GAV-->
    <!--G：代表企业标识，一般用包的前缀-->
    <groupId>com.qfedu</groupId>
    <!--A：代表项目标识-->
    <artifactId>fengmiwuye</artifactId>
    <!--V：代表项目版本。
        SNAPSHOT：快照版(最新版)，
        RELEASE：发布版(稳定版)，
        第一个数字：大版本(较大更改，eg：架构改变了)，
        第二个数字：小版本(较小更改，eg：扩展了功能)，
        第三个数字：bug修复量。
    -->
    <version>1.0.0</version>

    <!--配置当前项目的依赖-->
    <dependencies>
        <!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <version>8.0.32</version>
        </dependency>   
    </dependencies>
</project>
```

<img title="" src="images/2023-03-21-11-17-56-image.png" alt="" data-align="inline" width="925">

完整的 pom.xml：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd "> 

    <!-- 父项目的坐标。如果项目中没有规定某个元素的值，那么父项目中的对应值即为项目的默认值。
         坐标包括group ID，artifact ID和 version。 --> 
    <parent> 
        <!-- 被继承的父项目的构件标识符 --> 
        <artifactId>xxx</artifactId>
        <!-- 被继承的父项目的全球唯一标识符 -->
        <groupId>xxx</groupId> 
        <!-- 被继承的父项目的版本 --> 
        <version>xxx</version>
        <!-- 父项目的pom.xml文件的相对路径。相对路径允许你选择一个不同的路径。默认值是../pom.xml。
             Maven首先在构建当前项目的地方寻找父项目的pom，其次在文件系统的这个位置（relativePath位置），
             然后在本地仓库，最后在远程仓库寻找父项目的pom。 --> 
        <relativePath>xxx</relativePath> 
    </parent> 


    <!-- 声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，
         这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。 --> 
    <modelVersion> 4.0.0 </modelVersion> 
    <!-- 项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 
         如com.mycompany.app生成的相对路径为：/com/mycompany/app --> 
    <groupId>xxx</groupId> 
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有两个不同的项目拥有同样的artifact ID
         和groupID；在某个特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven
         为项目产生的构件包括：JARs，源码，二进制发布和WARs等。 --> 
    <artifactId>xxx</artifactId> 
    <!-- 项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型 --> 
    <packaging> jar </packaging> 
    <!-- 项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 --> 
    <version> 1.0-SNAPSHOT </version> 
    <!-- 项目的名称, Maven产生的文档用 --> 
    <name> xxx-maven </name> 
    <!-- 项目主页的URL, Maven产生的文档用 --> 
    <url> http://maven.apache.org </url> 
    <!-- 项目的详细描述, Maven 产生的文档用。 当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，
         就可以包含HTML标签）， 不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的
         索引页文件，而不是调整这里的文档。 --> 
    <description> A maven project to study maven. </description> 
     <!-- 项目创建年份，4位数字。当产生版权信息时需要使用这个值。 --> 
    <inceptionYear /> 


    <!-- 模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 --> 
    <modules>
        <!--子项目相对路径-->
        <module></module>
    </modules>

    <!-- 继承自该项目的所有子项目的默认依赖信息。这部分的依赖信息不会被立即解析,而是当子项目声明一个依赖
        （必须描述group ID和artifact ID信息），如果group ID和artifact ID以外的一些信息没有描述，则通过
            group ID和artifact ID匹配到这里的依赖，并使用这里的依赖信息。 --> 
    <dependencyManagement> 
        ......
    </dependencyManagement> 


    <!-- 构建项目需要的信息 --> 
    <build> 
        ......
    </build> 

    <!-- 发现依赖和扩展的远程仓库列表。 --> 
    <repositories> 
        ......
    </repositories>

    <!-- 在列的项目构建profile，如果被激活，会修改构建处理 --> 
    <profiles> 
        ......
    </profiles> 

    <!-- 发现插件的远程仓库列表，这些插件用于构建和报表 --> 
    <pluginRepositories> 
        ......
    </pluginRepositories> 

    <!-- 该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。
         要获取更多信息，请看项目依赖机制。 --> 
    <dependencies> 
        ......
    </dependencies> 
    <reporting> 
        ......
    </reporting> 

    <!-- 项目分发信息，在执行mvn deploy后表示要发布的位置。有了这些信息就可以把网站部署到远程服务器或者
         把构件部署到远程仓库。 --> 
    <distributionManagement> 
        ......
    </distributionManagement> 

    <!-- 以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件（见settings.xml配置文件里
         activation元素的说明）。格式是<name>value</name>。 --> 
    <properties>
        <name>value</name>
    </properties> 
</project> 
```

| 字段                 | 说明                                |
|:------------------:|:---------------------------------:|
| xmlns              | 命名空间，类似包名，因为xml的标签可自定义，需要命名空间来    |
| xmlns:xsi          | xml遵循的标签规范                        |
| xsi:schemaLocation | 用来定义xmlschema的地址，也就是xml书写时需要遵循的语法 |

<img src="images/2023-04-19-12-55-49-image.png" title="" alt="" width="956">

---

Maven 的项目结构是统一的，所以项目可以跨集成开发环境（IDE）进行开发。

---

依赖管理流程：

```java
public static void main(String[] args) {
    在项目中的 pom.xml 配置依赖;

    Maven 自动/手动 更新 pom.xml {

        Maven 读取项目的 pom.xml;

        if(Maven 检查本地仓库是否有项目所需的依赖(jar)，返回true/false) {
            直接将依赖引入(不是复制)到项目中;
        } else {
            Maven 会连接远程仓库下载依赖(需要网络);
            下载好的依赖放入本地仓库;
            将依赖从本地仓库引入(不是复制)到项目中;
        }
    }
}
```

# 六、Maven仓库

Maven 仓库：

- `本地仓库`：就是本地计算机上的某个文件夹（可以是自定义的任何文件夹）

- `远程仓库`：就是远程主机上的 jar 文件仓库
  
  - `中央仓库`：Maven 官方提供的仓库，包含了所需的一切依赖（免配置）
  
  - `公共仓库`：除了中央仓库以外的第三方远程仓库（需配置）。eg：阿里云仓库
  
  - `私服`：企业搭建的供内部使用的 Maven 仓库
    
    <img src="images/2023-03-21-13-40-18-image.png" title="" alt="" width="991">

---

仓库配置：(在 conf/settings.xml文件中配置)

+ 本地仓库配置：(默认 ${user.home}/.m2/repository 作为本地仓库)
  
  在settings.xml文件中有如下代码：
  
  ```xml
    <!-- localRepository
     | The path to the local repository maven will use to store artifacts.
     |
     | Default: ${user.home}/.m2/repository
    <localRepository>/path/to/local/repo</localRepository>
    -->
  ```
  
  将本地仓库路径写入 `<localRepository>路径</localRepository>`中即可。

+ 公共仓库配置：(在 conf/settings.xml文件中配置)
  
  在settings.xml文件中有如下代码：
  
  ```xml
  <!-- mirrors
     | This is a list of mirrors to be used in downloading artifacts from remote repositories.
     |
     | It works like this: a POM may declare a repository to use in resolving certain artifacts.
     | However, this repository may have problems with heavy traffic at times, so people have mirrored
     | it to several places.
     |
     | That repository definition will have a unique id, so we can create a mirror reference for that
     | repository, to be used as an alternate download site. The mirror site will be the preferred
     | server for that repository.
     |-->
    <mirrors>
      <!-- mirror
       | Specifies a repository mirror site to use instead of a given repository. The repository that
       | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
       | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
       |
      <mirror>
        <id>mirrorId</id>
        <mirrorOf>repositoryId</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://my.repository.com/repo/path</url>
      </mirror>
       -->
  
      <!--下述mirror要注销。
          因为使用HTTP协议下载依赖，可能会导致中间人攻击，
          所以Maven 3.8.1及以后版本就禁止了所有HTTP协议的Maven仓库。
          很多公司内部的maven仓库一般都是http协议，
          而Maven 3.8.1禁止了http协议，那么就会导致拉取maven报错。
      -->
      <mirror>
        <id>maven-default-http-blocker</id>
        <mirrorOf>external:http:*</mirrorOf>
        <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
        <url>http://0.0.0.0/</url>
        <blocked>true</blocked>
      </mirror>
    </mirrors>
  ```
  
  将 Maven 镜像地址写入`<mirrors>Maven镜像</mirrors>`中即可。
  
  阿里云Maven仓库地址：
  
  ```xml
  <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
  </mirror>
  ```

# 七、Maven项目生命周期管理

**Maven中存在三种生命周期：clean、default、site，分别用于清理项目、构建项目、生成项目站点。**

<img title="" src="images/2023-03-21-15-02-09-image.png" alt="" data-align="inline" width="863">

1. clean 生命周期的目的是清理项目，它包括以下三个阶段。
   
   + pre-clean：执行清理前需要完成的工作。
   
   + clean：清理上一次构建过程中生成的文件，比如编译后的 class 文件等。
   
   + post-clean：执行清理后需要完成的工作。

2. default 生命周期定义了构建项目时所需要的执行步骤，它是所有生命周期中最核心部分，包含的阶段如下表所述。
   
   | 阶段          | 处理   | 描述                           |
   |:-----------:|:----:|:----------------------------:|
   | 验证 validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的          |
   | 编译 compile  | 执行编译 | 源代码编译在此阶段完成                  |
   | 测试 Test     | 测试   | 使用适当的单元测试框架（例如JUnit）运行测试。    |
   | 包装 package  | 打包   | 创建JAR/WAR包如在 pom.xml 中定义提及的包 |
   | 检查 verify   | 检查   | 对集成测试的结果进行检查，以保证质量达标         |
   | 安装 install  | 安装   | 安装打包的项目到本地仓库，以供其他项目使用        |
   | 部署 deploy   | 部署   | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |

3. site 生命周期的目的是建立和发布项目站点。Maven 可以基于 pom 所描述的信息自动生成项目的站点，同时还可以根据需要生成相关的报告文档集成在站点中，方便团队交流和发布项目信息。site 生命周期包括如下阶段。
   
   + pre-site：执行生成站点前的准备工作。
   
   + site：生成站点文档。
   
   + post-site：执行生成站点后需要收尾的工作。
   
   + site-deploy：将生成的站点发布到服务器上。

项目生命周期管理常用指令：

+ `mvn clean`：清理缓存

+ `mvn check`：检查

+ `mvn compile`：编译

+ `mvn test`：测试

+ `mvn package`：打包

+ `mvn install`：安装

+ `mvn deploy`：部署

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.26</version>

    <!--解释在下方-->
    <scope>provided</scope>
</dependency>
```

scope标签的作用：

| 取值       | 使用范围        | 作用                                                                                   |
|:--------:|:-----------:|:------------------------------------------------------------------------------------:|
| system   | 编译期、测试期     | 由于版权限制等原因，不是所有的jar包都存储在中央仓库中。scope取值为system时，表示依赖来自本地系统，需要配合标签systemPath（用于指定依赖路径）使用 |
| runtime  | 测试期、运行期     | 测试和运行时需要                                                                             |
| provided | 编译期、测试期     | provided表示已提供，即打包期间不需要该依赖，该依赖会由其他依赖或容器会提供，避免依赖冲突                                     |
| test     | 测试期         | 用于测试代码的编译和执行，如：junit                                                                 |
| compile  | 编译期、测试期、运行期 | 默认                                                                                   |

1. maven项目依赖生效分为三个时期，编译期、测试期、运行期，其中scope默认取值为compile；

2. runtime 常用complie 代替，而compile是默认可以不用配置，也就是所一般只配test和provided；

<img title="" src="images/2023-03-21-20-22-30-image.png" alt="" width="848">

# 八、Maven私服

企业使用私服的优点：

1. 开发者需要依赖可直接从私服下载，从而屏蔽开发者与外网之间的直接交互

2. 可以对版本进行统一的管理

3. 可以将开发的依赖发布到私服，使得该依赖可以在企业内部实现共享

---

私服的搭建：

我们可以通过专门的Maven仓库管理软件来搭建私服。eg：Apache Archiva，Nexus。

安装私服 Nexus 3.x 流程：

<img src="images/2023-03-22-00-30-55-image.png" title="" alt="" width="962">

在浏览器中输入 `http://localhost:8081` 链接进入 Nexus Repository Manager (Nexus仓库管理器) 页面后，点击右上角的 Sign In 登录。如下图：

<img title="" src="images/2023-03-22-00-47-15-image.png" alt="" data-align="inline">

初始登录名称：`admin`

初始登录密码：在如下文件中：

| `nexus-3.49.0-02-win64\sonatype-work\nexus3\admin.password` |
|:-----------------------------------------------------------:|
| ![](images/2023-03-22-00-49-40-image.png)                   |

第一次登录进去后会显示重置密码：

| <img src="images/2023-03-22-00-52-21-image.png" title="" alt="" width="712"> |
|:----------------------------------------------------------------------------:|
| 将密码改为：123456789（随自己意愿改）                                                      |

查看 Nexus 仓库管理器中的仓库：

<img src="images/2023-03-22-01-09-18-image.png" title="" alt="" width="1010">

查看仓库的类型 (Type) 可知有：group、proxy、hosted、virtual。

+ group：不是一个具体的仓库，是一个仓库组（逻辑概念）

+ hosted：私服中的存储依赖的文件夹（物理概念，具体的仓库）

+ proxy：代理仓库，表示一个中央仓库/公共仓库（逻辑概念）

+ virtual：虚拟类型仓库

记住，不能直接连接 Nexus 仓库管理器中的 具体(hosted)仓库，因为如果本地仓库中没有目标依赖，Maven 就会到你连接的私服中的具体仓库中去寻找，如果你连接的私服仓库中也没有目标依赖，那就代表该依赖不存在，因为它不会到 中央仓库/公共仓库 中去寻找（私服仓库也是远程仓库）。

正确做法如下：

<img src="images/2023-03-22-01-31-03-image.png" title="" alt="" width="1141">

点击 maven-public 这个仓库组：

<img src="images/2023-03-22-01-34-37-image.png" title="" alt="" width="920">

查看 Group 下的内容可知，该仓库组有三个成员仓库 (maven-releases, maven-snapshots, maven-central)。（使用时需将左边 Available 的成员仓库添加到右边 Members 中）

<img src="images/2023-03-22-01-37-07-image.png" title="" alt="" width="455">

这三个成员仓库中有两个是具体仓库 (maven-releases, maven-snapshots)，一个是代理仓库 (maven-central)。

如果我们连接的是该仓库组(maven-public)，那么当本地仓库中没有依赖时，Maven就会到私服中，对该仓库组中的成员仓库依次进行依赖查找 (因为是依次查找，所以要实现先找具体仓库，再找代理仓库，那么就要将具体仓库放在代理仓库之前)，此时若我们将代理仓库连接到 中央仓库/公共仓库 上，那么当私服中的具体成员仓库中没有依赖时，Maven就会到代理仓库所连接的 中央仓库/公共仓库 上去找依赖。

所以我们连接私服时，应连接其 **仓库组**。

+ maven-releases：发布版依赖仓库

+ maven-snapshots：快照版依赖仓库

仓库组中成员仓库排放顺序：

1. maven-releases

2. maven-snapshots（可不要，只要保证仓库组中至少有一个具体仓库和代理仓库即可）

3. maven-central

代理仓库默认是中央仓库：

<img src="images/2023-03-22-01-53-24-image.png" title="" alt="" width="881">

可将其改成 阿里云公共仓库：`https://maven.aliyun.com/repository/public`

建议是不要改，我们可以再添加一个代理仓库，将其连接到 阿里云公共仓库，并将该代理仓库添加到仓库组中 (放在 具体仓库和中央仓库的代理仓库 之间)，这样的话当阿里云公共仓库没有目标依赖时，Maven 就会到中央仓库中去找。这样既保证了依赖下载速度 (外网速度好慢)，又保证了仓库的完整性。

如下图所示：

<img src="images/2023-03-22-02-13-12-image.png" title="" alt="" width="882">

下图为 仓库组(maven-public) 的URL：

<img src="images/2023-03-22-02-14-55-image.png" title="" alt="" width="517">

---

私服的配置：

1. 在 Maven 的 settings.xml 文件中的 `<servers>` 标签中配置私服的认证信息
   
   ```xml
   <servers>
       <!--配置连接私服所需的账号和密码-->
       <server>
           <!--nexus的认证id，随便写，但下文私服的配置要与其保持一致-->
           <id>nexus-public</id>
           <!--nexus中的用户名-->
           <username>admin</username>
           <!--nexus中的密码-->
           <password>123456789</password>
       </server>
   </servers>
   ```

2. 在 Maven 的 settings.xml 文件中的 `<profiles>` 标签中配置私服
   
   ```xml
   <profiles>
       <profile>
           <!--此处id随便-->
           <id>nexus</id>
   
           <repositories>
             <repository>
               <!--nexus认证id【此处的repository的id要和<server>的id保持一致】-->
               <id>nexus-public</id>
               <!--name随便-->
               <name>Nexus Release Snapshot Repository</name>
               <!--地址是nexus中仓库组对应的地址-->
               <url>http://localhost:8081/repository/maven-public/</url>
               <!--远程仓库中的releases仓库是否可用-->
               <releases>
                 <!--仓库可用-->
                 <enabled>true</enabled>
               </releases>
               <!--远程仓库中的snapshots仓库是否可用-->
               <snapshots>
                 <!--仓库可用-->
                 <enabled>true</enabled>
               </snapshots>
             </repository>
           </repositories>
   
           <!--插件仓库地址,各节点的含义和上面是一样的-->
           <pluginRepositories>
             <pluginRepository>
               <!--nexus认证id【此处的repository的id要和<server>的id保持一致】-->
               <id>nexus-public</id>
               <!--地址是nexus中仓库组对应的地址-->
               <url>http://localhost:8081/repository/maven-public/</url>
               <!--远程仓库中的releases仓库是否可用-->
               <releases>
                 <!--仓库可用-->
                 <enabled>true</enabled>
               </releases>
               <!--远程仓库中的snapshots仓库是否可用-->
               <snapshots>
                 <!--仓库可用-->
                 <enabled>true</enabled>
               </snapshots>
             </pluginRepository>
           </pluginRepositories>
   
         </profile>
   </profiles>
   ```

3. 在 Maven 的 settings.xml 文件中的 `<activeProfiles>` 标签中使私服配置生效
   
   ```xml
   <activeProfiles>
       <!--使私服配置生效，该值与配置的<profile>标签中的id值一致-->
       <activeProfile>nexus</activeProfile>
       <!--要激活哪个<profile>，就将要激活的<profile>的id值按上述标签写入此处-->
   </activeProfiles>
   ```

4. 在 Maven 的 settings.xml 文件中的 `<mirrors>` 标签中将镜像修改成私服镜像
   
   ```xml
   <mirrors>
       <mirror>
         <!--id要和<server>的id保持一致-->
         <id>nexus-public</id>
         <mirrorOf>*</mirrorOf>
         <name>nexus</name>
         <url>http://localhost:8081/repository/maven-public/</url>
       </mirror>
   </mirrors>
   ```

5. 当项目要部署 jar 包到私服上时，在项目的 pom.xml 文件中配置私服的仓库地址
   
   ```xml
   <!--在项目的pom.xml中配置私服的仓库地址,可以按项目打jar包部署到私服-->
   <distributionManagement>
       <repository>
           <!--nexus认证id-->
           <id>nexus-public</id>
           <url>http://localhost:8081/repository/maven-releases/</url>
       </repository>
       <snapshotRepository>
           <!--nexus认证id-->
           <id>nexus-public</id>
           <url>http://localhost:8081/repository/maven-snapshots/</url>
       </snapshotRepository>
   </distributionManagement>
   ```

# 九、Maven聚合工程
