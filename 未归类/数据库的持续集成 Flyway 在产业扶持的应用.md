# Flyway

# WHAT

> 持续数据库集成（Continuous Database Integration ,CDBI）即每次项目的版本控制库中发生变更时，重建数据库和测试数据。
> 数据库代码包括：DDL、DML、配置文件等。
> 在本质上与系统的其他源代码没有差别，实际上数据库的构建可以纳入CI系统，与数据库集成有关的制品应该放在版本控制库中，进行严格的测试和审查以确定是否遵守策略，并可通过构建脚本生成。

![image-20220929101534735](Flyway.assets/image-20220929101534735.png)



## WHY-为什么要使用Flyway

![img](Flyway.assets/Environments.png)

![img](Flyway.assets/SoftGreen.png)

我们在代码的版本管理方面经验十足，git、svn等版本管理工具功能强大，可以很好的帮助我们管理代码，我们有定义良好的发布和部署流程。

但是SQL脚本呢？

* 比如我如何知道某台机器上面的数据库处于什么状态？
* sql脚本文件是执行了，还是没有执行？
* 在生产环境环境的hotfix而执行的脚本文件随后在其他的环境有执行吗？
* 如何启动一台全新的数据库实例，而与其他的数据库实例保持一致？

![image-20221021142514437](Flyway.assets/image-20221021142514437.png)

## HOW-Flyway是如何工作的

Flyway工作流程如下：

1、项目启动，应用程序完成数据库连接池的建立后，Flyway自动运行。

2、初次使用时，Flyway会创建一个`flyway_schema_history`表，用于记录sql执行记录。

3、Flyway会扫描项目指定路径下(默认是`classpath:db/migration`)的所有sql脚本，与`flyway_schema_history`表脚本记录进行比对。如果数据库记录执行过的脚本记录，与项目中的sql脚本不一致，Flyway会报错并停止项目执行。

4、如果校验通过，则根据表中的sql记录最大版本号，忽略所有版本号不大于该版本的脚本。再按照版本号从小到大，逐个执行其余脚本。



## 基于 spring-boot 集成

1. 依赖导入

   pom.xml 配置

   ```xml
   <!-- flyway自动执行sql -->
   <dependency>
       <groupId>org.flywaydb</groupId>
       <artifactId>flyway-core</artifactId>
       <version>${flyway.version}</version>
   </dependency>
   ```

2. 配置文件

   application-flyway.yml

   ```yaml
   spring:
     flyway:
       # 是否启用flyway
       enabled: true
       # 禁止删除库！！！
       clean-disabled: true
       # 编码格式，默认UTF-8
       encoding: UTF-8
       # 迁移sql脚本文件存放路径，默认db/migration
       locations: classpath:db/migration
       # 迁移sql脚本文件名称的前缀，默认V
       sql-migration-prefix: V
       # 迁移sql脚本文件名称的分隔符，默认2个下划线__
       sql-migration-separator: __
       # 迁移sql脚本文件名称的后缀
       sql-migration-suffixes: .sql
       # 迁移时是否进行校验，默认true
       validate-on-migrate: true
       # 当迁移发现数据库非空且存在没有元数据的表时，自动执行基准迁移，新建schema_version表
       baseline-on-migrate: true
   ```

3. 脚本与目录的命名规则

   * 以版本号命名目录；
   * 以”V日期_三位序号__jira单号+注释“命名脚本；例子如下图。

   ![image-20220929100754447](Flyway.assets/image-20220929100754447.png)

试验并观察结果

![image-20220929100606267](Flyway.assets/image-20220929100606267.png)

![image-20220929101135645](Flyway.assets/image-20220929101135645.png)



## 注意要点 & 项目规范：

* version后面的下划线是两个下划线；
* migration 后的SQL 脚本不能再次修改，不然启动程序的时候会报错；
* 脚本注意编码格式，建议UTF8（根据项目配置）；
* 版本的V的字母注意大小写（根据项目配置）；
* migration目录下至少存有一个文件；
* 开发要有及时保存脚本文件的意识，避免转测试时脚本遗漏。（提醒，并不涉及 flyway 功能）

- 额外提醒：库表结构等DDL需要在PDManer操作后，再保存到脚本，再由flyway执行。（定奇补充）



## FAQ：

Q：是不是每次有新的sql，都要重启一下这个项目？
A：其实这个角度来说，你应该把数据库管理和日常的项目操作数据分开。flywaydb原则上只管前者。

Q：开发环境下，重启时会重复执行开发导出的SQL，与现有数据库数据冲突。
A：一个解释，从数据库导出的SQL需要验证，开发需先把对应的变化（表对象或者数据）在数据库逆向撤回（删除表或者记录或者表结构），再从flyway里验证。

Q：脚本提交后才发现序号冲突，或者序号如何保证不冲突。
A：及时提交，占坑

Q：修改执行过的脚本
A：如果因为某些特定原因必须修改已经执行过的脚本，不修改不行，记住修改后脚本的`checksum`，手动覆盖到每一个环境的flyway_schema_history版本记录中。

Q：删除执行过的脚本
A：由于特殊原因必须删除已经执行过的脚本，例如sql脚本文件过多，想要清理，建议在所有环境同步更新后，再删除sql脚本文件，并对应删除flyway_schema_history里的版本记录。

Q：常见错误：Migration checksum mismatch for migration version 20220101.1
A：已经执行过的脚本被改动导致校验失败，还原即可。



## REF：

[Flyway基本介绍及基本使用-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/842712)

[flyway使用教程 - 简书 (jianshu.com)](https://www.jianshu.com/p/3f762f6ef5c1)

[flyway - 文集 - 简书 (jianshu.com)](https://www.jianshu.com/nb/45067095)

[Flyway：数据库版本迁移工具的介绍 - 一尾金鱼 - 博客园 (cnblogs.com)](https://www.cnblogs.com/ywjy/p/10959475.html)

[SpringBoot中使用Flyway_一个小坑货的博客-CSDN博客_flyway springboot](https://blog.csdn.net/qq_35995691/article/details/122770451)

[How Flyway works - Flyway by Redgate • Database Migrations Made Easy. (flywaydb.org)](https://flywaydb.org/documentation/getstarted/how)