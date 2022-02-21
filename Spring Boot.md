# **Spring Boot 笔记 ** 

## 建立 Spring Boot工程(使用Gradle 构建)

```gro
plugins {
	id 'org.springframework.boot' version '2.5.0'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.ding'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web' // 添加此依赖 Web Starter
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

test {
	useJUnitPlatform()
}
```

