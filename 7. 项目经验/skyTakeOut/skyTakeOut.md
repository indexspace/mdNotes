# Knife4j (swagger增强解决方案)

## 依赖

```xml
<dependency>
		<groupId>com.github.xiaoymin</groupId>
		<artifactId>knife4j-spring-boot-starter</artifactId>
		<version>${knife4j}</version>
</dependency>
```

## 配置

WebMvcConfiguration

```java
@Configuration
@Slf4j
public class WebMvcConfiguration extends WebMvcConfigurationSupport {
    @Bean
    public Docket docket() {
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("苍穹外卖项目接口文档")
                .version("2.0")
                .description("苍穹外卖项目接口文档")
                .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }

    /**
     * 设置静态资源映射
     * @param registry
     */
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
}
```

## 测试

web访问:

```basic
localhost:8080/doc.html
```





# 新增员工接口

## Controller

```java
package com.sky.controller.admin;

import com.sky.dto.EmployeeDTO;
import // 其他package

@RestController
@RequestMapping("/admin/employee")
@Slf4j
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;
    
    // 其他code

    @PostMapping
    @ApiOperation("新增员工")
    public Result save(@RequestBody EmployeeDTO employeeDTO) {
        log.info("新增员工: {}", employeeDTO);
        employeeService.save(employeeDTO);
        return Result.success();
    }

}
```

## Server

```java
import com.sky.dto.EmployeeDTO;
import // 其他package
    
public interface EmployeeService {

    // 其他code
    
    void save(EmployeeDTO employeeDTO);

}
```

## ServerImpl

```java
package com.sky.service.impl;

import org.springframework.beans.BeanUtils;
import org.springframework.util.DigestUtils;
import // 其他package

@Service
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeMapper employeeMapper;

    // 其他code

    public void save(EmployeeDTO employeeDTO) {
        Employee employee = new Employee();
        BeanUtils.copyProperties(employeeDTO, employee);
        employee.setPassword( DigestUtils.md5DigestAsHex(
            PasswordConstant.DEFAULT_PASSWORD.getBytes()) );
        employee.setStatus(StatusConstant.ENABLE);
        employee.setCreateTime(LocalDateTime.now());
        employee.setUpdateTime(LocalDateTime.now());
        employee.setCreateUser(10L);
        employee.setUpdateUser(10L);

        employeeMapper.insert(employee);
    }

}
```

## Mapper

```java
package com.sky.mapper;

import com.sky.entity.Employee;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface EmployeeMapper {

    @Insert("insert into employee (name, username, password, phone, sex, id_number, status, create_time, update_time,  create_user, update_user) values (#{name}, #{username}, #{password}, #{phone}, #{sex}, #{idNumber}, #{status}, #{createTime}, #{updateTime}, #{createUser}, #{updateUser})")
    void insert(Employee employee);

}
```

