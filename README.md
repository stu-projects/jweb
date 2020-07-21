# jweb
Simple java web build via maven

## Example settings.xml to use
```
<?xml version="1.0" encoding="UTF-8"?>
<settings>
        <servers>
                <server>
                        <id>nexus-snapshots</id>
                        <username>admin</username>
                        <password>*add password*</password>
                </server>
                <server>
                        <id>nexus-releases</id>
                        <username>admin</username>
                        <password>*add password*</password>
                </server>
                <server>
                        <id>nexus</id>
                        <username>admin</username>
                        <password>*add password*</password>
                </server>
        </servers>
        <mirrors>
                <mirror>
                        <!--This sends everything else to /public -->
                        <id>nexus</id>
                        <mirrorOf>*</mirrorOf>
                        <url>https://*nexus server name*/repository/maven-central/</url>
                </mirror>
        </mirrors>
        <profiles>
                <profile>
                        <id>nexus</id>
                        <!--Enable snapshots for the built in central repo to direct -->
                        <!--all requests to nexus via the mirror -->
                        <repositories>
                                <repository>
                                        <id>central</id>
                                        <url>http://central</url>
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
                                        <id>central</id>
                                        <url>http://central</url>
                                        <releases>
                                                <enabled>true</enabled>
                                        </releases>
                                        <snapshots>
                                                <enabled>true</enabled>
                                        </snapshots>
                                </pluginRepository>
                        </pluginRepositories>
                </profile>
        </profiles>
        <activeProfiles>
                <!--make the profile active all the time -->
                <activeProfile>nexus</activeProfile>
        </activeProfiles>
</settings>

```

