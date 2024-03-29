### 新增项目
复制model-config/gservice到service目录，修改服务名称，编辑schema.json，生成服务项目
```
gservice=mobile java -jar codegen-cli.jar -f light-hybrid-4j-service -c service/${gservice}/config.json -m service/${gservice}/schema.json -o service/${gservice}
```

### pom.xml，gservice自身依赖可以参考ip和服务一起打包，gserver公共依赖参考bank可以直接引用mysql数据源
mvn eclipse:eclipse -f service/pom.xml
```
<dependency>
    <groupId>com.xlongwei</groupId>
    <artifactId>gserver</artifactId>
    <version>3.0.1</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.8</version>
    <scope>provided</scope>
</dependency>
```

### 调试gservice：继承HybridHandler，支持init初始化，使用HybridUtils返回中文json
```
{
    "type": "java",
    "name": "apijson",
    "request": "launch",
    "mainClass": "com.networknt.server.Server",
    "projectName": "apijson",
    "cwd": "${workspaceFolder}/light-service",
    "vmArgs": "-Dlight-4j-config-dir=gserver/src/main/resources/config -Dlogback.configurationFile=gserver/src/test/resources/logback-test.xml",
}
```


### vscode
mvn package -DskipTests -Dmaven.javadoc.skip=true -f service/pom.xml
```
{
    "type": "java",
    "name": "gserver",
    "request": "launch",
    "mainClass": "com.networknt.server.Server",
    "projectName": "gserver",
    "cwd": "${workspaceFolder}/light-service",
    "vmArgs": "-Dlight-4j-config-dir=gserver/src/main/resources/config -Dlogback.configurationFile=gserver/src/test/resources/logback-test.xml",
    "classPaths": [
        "E:\\GITHUB\\xlongwei\\light-service\\gserver\\target\\gserver-3.0.1.jar",
        "E:\\GITHUB\\xlongwei\\light-service\\gservice\\target\\gservice-3.0.1.jar",
        "E:\\GITHUB\\xlongwei\\light-service\\service\\bank\\target\\bank-3.0.1.jar",
        "E:\\GITHUB\\xlongwei\\light-service\\service\\id\\target\\id-3.0.1.jar",
        "E:\\GITHUB\\xlongwei\\light-service\\service\\ip\\target\\ip-3.0.1.jar",
    ],
    "env": {
        "id.orderBy":"pinyin"
    }
}
```

### git-bash
```
sh start.sh package
sh start.sh dist
sh start.sh start
```
