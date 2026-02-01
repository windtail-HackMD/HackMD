# Redis Stream

## Docker安裝Redis
```
docker run -d --name redis6 -p 6379:6379 redis:6.0
```
![image](https://hackmd.io/_uploads/HkRApqoL-e.png)

測試 Redis 有沒有起來，看到PONG就成功了
```
docker exec -it redis6 redis-cli

ping
```
![image](https://hackmd.io/_uploads/SyNo0ci8-g.png)

離開Redis Shell打exit指令即可
```
exit
```

## 建立SpringBoot
### pom.xml
pom.xml（含 Lombok、Redis、Redisson 相容）
- Spring Boot 2.2.1.RELEASE
- Java 11
- Redis 6（Docker）
- Redisson 已存在（不衝突）
- 使用 Lombok

1. 使用Lombok專案記得啟用以下設定
右鍵專案 → Properties → Java Compiler → Annotation Processing。
勾選 Enable annotation processing。
![image](https://hackmd.io/_uploads/B1S0gBhLbg.png)


2.如果遇到Lombok 沒有生效(編譯器標記程式碼錯誤)，Maven install卻成功，代表編譯器不知道lombok，請按照以下設定
(建議用 JDK 17 或 JDK 21 LTS，搭配 Lombok 1.18.42。)
```
手動改 SpringToolsForEclipse.ini(用「記事本（系統管理員）」打開，不然存不了)
C:\springboot-eclipse\SpringToolsForEclipse.ini
```
在 -vmargs 下面 加這一行
(-javaagent 一定要在 -vmargs 之後)
```
-javaagent:C:\Users\4a015\.m2\repository\org\projectlombok\lombok\1.18.42\lombok-1.18.42.jar
```
存檔 → 完全關閉 STS → 重開
![image](https://hackmd.io/_uploads/SJx-CBhUZe.png)


```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>file-scan-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
    </parent>

    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>

        <!-- Spring Boot 基本 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <!-- Redis -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <!-- Redisson（你原本就有） -->
        <dependency>
            <groupId>org.redisson</groupId>
            <artifactId>redisson-spring-boot-starter</artifactId>
            <version>3.16.7</version>
        </dependency>

        <!-- Lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- Test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

### application.yml
```
server:
  port: 8080

spring:
  redis:
    host: localhost
    port: 6379
    timeout: 3000ms
    lettuce:
      pool:
        max-active: 16
        max-idle: 8
        min-idle: 1

logging:
  level:
    root: INFO

```
![image](https://hackmd.io/_uploads/S1HKoE2IZg.png)

### 程式碼
FileScanDemoApplication.java
```
@SpringBootApplication
@EnableScheduling
public class FileScanDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(FileScanDemoApplication.class, args);
    }
}
```