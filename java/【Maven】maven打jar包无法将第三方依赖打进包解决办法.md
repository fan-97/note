在pom文件里面添加插件

```xml

<build>
    <plugins>
       <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>2.4.1</version>
                <configuration>
                    <!-- get all project dependencies -->
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <!-- MainClass in mainfest make a executable jar -->
                    <archive>
                        <manifest>
                            <mainClass>com.fanjie.wc.StreamWordCount</mainClass>
                        </manifest>
                    </archive>

                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <!-- bind to the packaging phase -->
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
    </plugins>
</build>
```

![image-20210629230922087](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20210629230929.png)



assembly:assembly 打包即可。



FlinkTutorial-1.0-SNAPSHOT-exec.jar：不含第三方依赖

FlinkTutorial-1.0-SNAPSHOT-jar-with-dependencies.jar：含第三方依赖