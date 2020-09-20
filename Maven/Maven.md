# Maven

## Maven问题

### maven打包为jar文件时，scope为system的jar包无法被打包进jar文件
1. 问题描述
<br/>scope为system的maven默认是不打包进去的。
2. 解决方法
<br/>\<plugin>里添加\<includeSystemScope>配置。
```
<plugins>
	<plugin>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-maven-plugin</artifactId>
		<configuration>
			<includeSystemScope>true</includeSystemScope>
		</configuration>
	</plugin>
</plugins>
```
3. 参考
* https://github.com/spring-projects/spring-boot/issues
* https://www.cnblogs.com/musarona/p/11204179.html




