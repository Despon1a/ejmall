![image2](http://ouip1glzq.bkt.clouddn.com/blog/20180807083943.png)

用debug模式本地启动就可以了。

客户端端口统一配置，如下：
* front:    http://localhost:8080
* h5:       http://localhost:8081
* seller:   http://localhost:8082
* admin:    http://localhost:8083

## 二、开发、测试环境的区分

各个环境的区分使用maven的profile。

* 本地环境：dev
* 测试环境：test
* 生产环境：prod

### 开发环境配置

开发环境中，所有的服务提供者**不会**向zk注册中心注册，通过如下配置完成：
```xml
<dubbo:registry protocol="${pom.registry.protocol}" address="${pom.registry.address}" register="${pom.registry.register}"/>
```
其中，register统一配置为false，即不会向注册中心注册。

客户端中，每一个服务都直连本地的服务，不会从注册中心去取服务，配置如下：
```xml
<dubbo:reference interface="com.ejmall.service.system.ICodeService" id="codeService" retries="0" url="${pom.dubbo.direct.url}" />
```
其中url指向本地地址：dubbo://localhost:20901

这样在本地启动，服务端不会向注册中心注册，客户端也不会从注册中心去取服务。
