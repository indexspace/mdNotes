# 简介

> 比较流行的**API**框架;
>
> API**文档**在线**自动生成**工具   *// API文档和API定义同步*
>
> 可以**在线测试**API**接口**



# SpringBoot集成swagger

1. 建立并运行SpringBoot项目的Demo

1. 导入swagger的依赖

    ```xml
    <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
    ```

1. 编写swagger的config文件

    ![image-20230614114910243](./image-20230614114910243.png)

1. 测试运行: `http://localhost:8080/swagger-ui.html`

	![image-20230616114136216](./image-20230616114136216.png)

> **! 注意**:
>
> 2.6.1 的 springboot 不支持 2.9.2 的 swagger 配置，报空指针错误
>
> 在 `application.properties`中加入
> ```properties
> spring.mvc.pathmatch.matching-strategy=ANT_PATH_MATCHER
> ```
> 即可




# 配置swagger



![image-20230616114842165](./image-20230616114842165.png)
