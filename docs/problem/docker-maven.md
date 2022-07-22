# Docker 搭建maven私服

<img src="./images/NexusRepoMngr_withSonatype@3x.png" style="width:350px;margin-top:-70px;float: right" class="no-zoom" />


##### 1. 增加docker-compose文件配置

```yaml
nexus:
    # 容器名称(随意起,见名知意)
    container_name: nexus_container
    # 指定基础镜像
    image: sonatype/nexus3
    # 容器崩了自动重启
    restart: always
    # 映射端口-nexus的默认端口是8081
    ports:
      - '8081:8081'
    # 数据卷的映射
    volumes:
      - ./nexus3/nexus-data:/nexus-data/
```

##### 2. 访问Nexus私服地址

> 启动之后可以进入容器里面查看nexus启动日志，没有报错的话稍等一分钟左右，通过：`http://IP:8081` 可以访问nexus管理界面。

!> 初始的登录用户名为：`admin`，初始密码：进入容器后`cat /nexus-data/admin.password` 查看

##### 3. 修改本地Maven配置

##### a. 修改`settings.xml`配置文件，增加身份认证信息

```xml
    <servers>
        <server>
            <id>nexus-releases</id>
            <username>admin</username>
            <password>密码</password>
        </server>
        <server>
            <id>nexus-snapshots</id>
            <username>admin</username>
            <password>密码</password>
        </server>
    </servers>
```

##### b. 增加发布管理节点

```xml
    <mirrors>
        <!--内部maven-->
        <mirror>
            <id>central</id>
            <mirrorOf>*</mirrorOf>
            <name>Central Repository</name>
            <url>http://IP:8081/repository/maven-public/</url>
        </mirror>
        <!--阿里云-->
        <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </mirror>
        <!-- 中央仓库1 -->
        <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo1.maven.org/maven2/</url>
        </mirror>
        <!-- 中央仓库2 -->
        <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
        </mirror>
    </mirrors>
```

##### 3. 通过IDEA发布jar包，至Maven仓库

a. 修改需要发布的项目的 pom.xml，指定发布仓库

```xml
<distributionManagement>
    <repository>
        <id>nexus-releases</id>
        <name>Nexus Release Repository</name>
        <url>http://ip:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <name>Nexus Snapshot Repository</name>
        <url>http://ip:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```
b. 根据情况指定发布版本

```xml
	<version>0.0.1-SNAPSHOT</version>
<!--	<version>0.0.1-RELEASE</version>-->
```

>如何发布源码至maven仓库
```xml
<build>
    <plugins>
        <!--   要将源码放上去，需要加入这个插件    -->
        <plugin>
            <artifactId>maven-source-plugin</artifactId>
            <version>3.0.1</version>
            <configuration>
                <attach>true</attach>
            </configuration>
            <executions>
                <execution>
                    <phase>compile</phase>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

c. 通过maven插件，发布项目。

<img src="./images/maven.png" style="width:350px" />
