# SpringBoot整合Swagger2

## 引入依赖
```
	<!-- swagger2 start -->
	<dependency>
		<groupId>io.springfox</groupId>
		<artifactId>springfox-swagger2</artifactId>
		<version>2.6.1</version>
	</dependency>

	<dependency>
		<groupId>io.springfox</groupId>
		<artifactId>springfox-swagger-ui</artifactId>
		<version>2.6.1</version>
	</dependency>
	<!-- end -->
```

## Swagger配置类
```
package com.zm.oa.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

@Configuration
public class Swagger2 {
	
	@Bean
	public Docket createRestApi() {
		return new Docket(DocumentationType.SWAGGER_2)
				.apiInfo(apiInfo())
				.select()
				.apis(RequestHandlerSelectors.basePackage("com.zm.oa.controller"))
				.paths(PathSelectors.any())
				.build();
	}
	
	private ApiInfo apiInfo() {
		return new ApiInfoBuilder()
				.title("OA 系统  api文档")
				.description("OA系统相关文档")
				.termsOfServiceUrl("")
				.version("1.0")
				.build();
	}
	
}

```
## 项目启动类加上Swagger2注解
```
package com.zm.oa;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import springfox.documentation.swagger2.annotations.EnableSwagger2;

@SpringBootApplication
@EnableSwagger2
public class OaApplication {

	public static void main(String[] args) {
		SpringApplication.run(OaApplication.class, args);
	}

}
```
## Swagger2 API文档
1. @API
```
@Api(value="员工Controller类",tags= {"员工Controller类Tag1","员工Controller类Tag1"})

```
