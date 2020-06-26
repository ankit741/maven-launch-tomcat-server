# maven-launch-tomcat-server
Maven Configuration to copy, launch and deploy war file on tomcat server with custom deployment folder.

# Usage

  ```  <properties>
        <dev.root.dir>${project.basedir}</dev.root.dir>
        <dev.dep.dir>deploy\tomcat</dev.dep.dir>
        <dev.wars.dir>deploy\tomcat\webapps</dev.wars.dir>
    </properties>
    <dependencies>
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>service</artifactId>
            <version>${project.version}</version>
            <type>war</type>
            <scope>provided</scope>
        </dependency>
    </dependencies>
 <build>
        <plugins>
            <plugin>
                <artifactId>maven-clean-plugin</artifactId>
                <version>3.1.0</version>
                <configuration>
                    <filesets>
                        <fileset>
                            <directory>${dev.wars.dir}</directory>
                            <includes>
                                <include>**/*</include>
                            </includes>
                        </fileset>
                    </filesets>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>copy-war-files</id>
                        <goals>
                            <goal>copy-dependencies</goal>
                        </goals>
                        <configuration>
                            <includeTypes>war</includeTypes>
                            <stripClassifier>false</stripClassifier>
                            <stripVersion>true</stripVersion>
                            <outputDirectory>${dev.wars.dir}</outputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-antrun-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>compile</phase>
                        <configuration>
                            <tasks>
                                <exec dir="${project.basedir}"
                                        executable="${project.basedir}/deploy/start.bat"
                                        failonerror="true">
                                    <arg line="${dev.root.dir}\${dev.dep.dir}" />
                                </exec>
                            </tasks>
                        </configuration>
                        <goals>
                            <goal>run</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```
# License

Licensed under Apache 2.0. Please see [LICENSE](LICENSE) for details.
