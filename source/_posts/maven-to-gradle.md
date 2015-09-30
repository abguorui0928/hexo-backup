title: maven to gradle
date: 2015-09-30 13:53:47
tags: [maven, gradle]
---
时下，Gradle项目构建越来越受欢迎，其灵活的配置确实要比maven强大，这里对比Maven与Gradle常用配置，以更快的掌握gradle，文章中的maven配置取材于我的开源项目fastdb，github地址：https://github.com/abguorui0928/fastdb
### 工程配置
#### Gradle工程配置
	apply plugin: 'java'
	apply plugin: 'idea'
	apply plugin: 'application'
	
	//default buildDir is build
	buildDir = "target"
	
	jar {
	    group = 'org.fastdb'
	    baseName = 'fastdb'
	    version = 0.1
	}
执行gradle idea生成idea环境下工程配置文件
#### Maven工程配置
	<project>
		<modelVersion>4.0.0</modelVersion>
		<groupId>org.fastdb</groupId>
		<artifactId>fastdb</artifactId>
		<version>0.1</version>
	</project>

执行mvn idea:idea生成idea环境下工程配置文件

### 仓库配置
#### Gradle仓库配置

    repositories {
	    mavenLocal()
	    maven {
	        name 'uk-nexus'
	        url "http://uk.maven.org/maven2"
	    }
	}
#### Maven仓库配置
	<repositories>
		<repository>
			<id>uk-nexus</id>
			<name>uk-nexus Repository</name>
			<url>http://uk.maven.org/maven2</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</repository>
	</repositories>
	<pluginRepositories>
		<pluginRepository>
			<id>uk-nexus</id>
			<name>uk-nexus Repository</name>
			<url>http://uk.maven.org/maven2</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>

### 依赖配置
#### Gradle依赖配置
    // build a map of the dependency artifacts to use.  Allows centralized definition of the version of artifacts to
	// use.  In that respect it serves a role similar to <dependencyManagement> in Maven
	ext {
	    c3p0Version = '0.9.5'
	    slf4jVersion = '1.7.5'
	    persistenceVersion = '1.0.2'
	    javassistVersion = '3.18.1-GA'
	    junitVersion = '4.10'
	    log4j12Version = '1.7.7'
	    h2databaseVersion = '1.4.187'
	    libraries = [
	            c3p0       : "com.mchange:c3p0:${c3p0Version}",
	            slf4j      : "org.slf4j:slf4j-api:${slf4jVersion}",
	            persistence: "javax.persistence:persistence-api:${persistenceVersion}",
	            javassist  : "org.javassist:javassist:${javassistVersion}",
	            junit      : "junit:junit:${junitVersion}",
	            log4j12    : "org.slf4j:slf4j-log4j12:${log4j12Version}",
	            h2database : "com.h2database:h2:${h2databaseVersion}"
	    ]
	}
	
	dependencies {
	    compile libraries.c3p0
	    compile libraries.slf4j
	    compile libraries.persistence
	    compile libraries.javassist
	    testCompile libraries.junit
	    testCompile libraries.log4j12
	    testCompile libraries.h2database
	}
#### Maven依赖配置
	<dependencies>
		<dependency>
			<groupId>com.mchange</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.5</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.7.5</version>
		</dependency>
		<dependency>
			<groupId>javax.persistence</groupId>
			<artifactId>persistence-api</artifactId>
			<version>1.0.2</version>
		</dependency>
		<dependency>
			<groupId>org.javassist</groupId>
			<artifactId>javassist</artifactId>
			<version>3.18.1-GA</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.10</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.7</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<version>1.4.187</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
### 编译器版本、编码配置
#### Gradle编译器版本、编码配置
	sourceCompatibility = 1.6
	targetCompatibility = 1.6
	
	compileJava.options.encoding = 'UTF-8'
	compileTestJava.options.encoding = 'UTF-8'
#### Maven编译器版本、编码配置
	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-compiler-plugin</artifactId>
		<version>3.1</version>
		<configuration>
			<source>1.6</source>
			<target>1.6</target>
			<encoding>UTF-8</encoding>
		</configuration>
	</plugin>
	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-resources-plugin</artifactId>
		<version>2.6</version>
		<configuration>
			<encoding>UTF-8</encoding>
		</configuration>
	</plugin>
### 可执行jar入口配置
#### Gradle可执行jar入口配置

	mainClassName = 'org.fastdb.Application'
#### Maven可执行jar入口配置
	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-jar-plugin</artifactId>
		<version>2.4</version>
		<configuration>
			<archive>
				<manifest>
					<addClasspath>true</addClasspath>
					<classpathPrefix>lib/</classpathPrefix>
					<mainClass>org.fastdb.Application</mainClass>
				</manifest>
			</archive>
		</configuration>
	</plugin>
### 常见错误
> 警告: [options] 未与 -source 1.6 一起设置引导类路径

该错误是由于执行gradle命令使用的JDK版本高于build.gradle中设定的sourceCompatibility的版本。