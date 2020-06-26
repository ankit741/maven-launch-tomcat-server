# maven-launch-tomcat-server
Maven Configuration to copy, launch and deploy war file on tomcat server with custom deployment folder.

# Update tomcat configuration 

Please follow below steps :

1.You should ensure that tomcat is installed and environment variables are set for tomcat and java .
2. Copy all tomcat conf files(**C:\apache-tomcat\conf**) to **dev\deploy\tomcat\conf** directory.
3. create below directories under **deploy\tomcat**
    conf
    logs
    temp
    webapps
    work

# start.bat
``
@ECHO OFF
SET TITLE=Tomcat - deploy service - port: 9090
set argument1=%1

SETLOCAL

set OC_DEVENV=%argument1%

set CLIENT_HOME=%OC_DEVENV%\webapps

set CATALINA_OPTS=%CATALINA_OPTS% -Dclient.home=%CLIENT_HOME%

set CATALINA_TMPDIR=%CATALINA_BASE%\temp

set JPDA_OPTS=-agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n

call %CATALINA_HOME%\bin\catalina.bat jpda start
```
# Usage

  ```  
  <properties>
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

# Start Tomcat 

mvn clean install

# License

Licensed under Apache 2.0. Please see [LICENSE](LICENSE) for details.
