```
<groupId>com.lumk</groupId>
<artifactId>sparkDemo</artifactId>
<version>1.0-SNAPSHOT</version>

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <encoding>UTF-8</encoding>
    <scala.version>2.10.6</scala.version>
</properties>

<dependencies>
    <!-- https://mvnrepository.com/artifact/org.apache.spark/spark-core_2.11 -->
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-core_2.11</artifactId>
        <version>1.6.3</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.spark/spark-sql_2.11 -->
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-sql_2.11</artifactId>
        <version>1.6.3</version>
    </dependency>
</dependencies>

<build>
    <finalName>spark-demo</finalName>
    <sourceDirectory>src/main/scala</sourceDirectory>
    <testSourceDirectory>src/test/scala</testSourceDirectory>
    <plugins>
        <!-- 使用assembly插件打包 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <!-- 打包结果名称末尾是否包含assemblyId -->
                <appendAssemblyId>false</appendAssemblyId>
                <!-- 打包时包含依赖包 -->
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
                <archive>
                    <manifest>
                        <mainClass>demo.Demo</mainClass>
                    </manifest>
                </archive>
            </configuration>
            <executions>
                <execution>
                    <id>make-assembly</id>
                    <phase>package</phase>
                    <goals>
                        <goal>assembly</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        <!-- 使用jar插件打包 -->
        <plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-jar-plugin</artifactId>
			<version>2.4</version>
			<configuration>
				<archive>
					<addMavenDescriptor>false</addMavenDescriptor>
					<manifest>
						<mainClass>demo.Demo</mainClass>
					</manifest>
				</archive>
			</configuration>
		</plugin>
    </plugins>
</build>

</project>
```