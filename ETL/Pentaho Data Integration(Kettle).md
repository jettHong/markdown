Pentaho Data Integration(Kettle) 

## **简介：**

Kettle 是一款国外开源的 ETL 工具，纯 Java 编写，绿色无需安装，数据抽取高效稳定(数据迁移工具)。Kettle 中有两种脚本文件，transformation 和 job，transformation 完成针对数据的基础转换，job 则完成整个工作流的控制。

Kettle 中文名称叫水壶，该项目的主程序员MATT 希望把各种数据放到一个壶里，然后以一种指定的格式流出。

Kettle这个ETL工具集，它允许你管理来自不同数据库的数据，通过提供一个图形化的用户环境来描述你想做什么，而不是你想怎么做。

![image-20220224214744340](Pentaho Data Integration(Kettle).assets/image-20220224214744340.png)

Kettle家族目前包括4个产品：Spoon、Pan、CHEF、Kitchen。 

SPOON 允许你通过图形界面来设计ETL转换过程（Transformation）。 

PAN 允许你批量运行由Spoon设计的ETL转换 (例如使用一个时间调度器)。Pan是一个后台执行的程序，没有图形界面。 

CHEF 允许你创建任务（Job）。 任务通过允许每个转换，任务，脚本等等，更有利于自动化更新数据仓库的复杂工作。任务通过允许每个转换，任务，脚本等等。任务将会被检查，看看是否正确地运行了。 

KITCHEN 允许你批量使用由Chef设计的任务 (例如使用一个时间调度器)。KITCHEN也是一个后台运行的程序。



## 下载与安装：

官网：[Hitachi Vantara - Data Storage and Analytics, DataOps, IoT, Cloud, Consulting](https://www.hitachivantara.com/en-us/home.html)

需要自行安装Java环境。

如果需要数据库连接的话，需要将驱动放入程序 lib 目录下。

![image-20220224215111291](Pentaho Data Integration(Kettle).assets/image-20220224215111291.png)

点击 Spoon.bat 运行。



## 操作示例

本例中，预期将excel数据导入数据库中。

提前准备好excel文件、创建相应数据库表，配置相应数据库连接驱动。

### 1、新建转换

![image-20220224215409976](Pentaho Data Integration(Kettle).assets/image-20220224215409976.png)

### 2、输入输出配置

#### 在【核心对象-输入】中，查到excel输入，并拖入转换面板中。

![image-20220224215507638](Pentaho Data Integration(Kettle).assets/image-20220224215507638.png)

双击表输入进行配置。

![image-20220224215937002](Pentaho Data Integration(Kettle).assets/image-20220224215937002.png)



进行工作表配置，点击获取工作表名称，取得表字段名。

![image-20220224220017775](Pentaho Data Integration(Kettle).assets/image-20220224220017775.png)

点击字段页签，点击获取来自头部数据的字段，取得相应字段。

![image-20220224220138871](Pentaho Data Integration(Kettle).assets/image-20220224220138871.png)

点击预览可以进行数据预览。

![image-20220224220305467](Pentaho Data Integration(Kettle).assets/image-20220224220305467.png)



按住Shift，手动输入指向输出。

![image-20220224220655892](Pentaho Data Integration(Kettle).assets/image-20220224220655892.png)



#### 在【核心对象-输出】中，查到表输出，并拖入转换面板中。

![image-20220224215717786](Pentaho Data Integration(Kettle).assets/image-20220224215717786.png)

![image-20220224220537343](Pentaho Data Integration(Kettle).assets/image-20220224220537343.png)

### 3、运行与检查

![image-20220224220710975](Pentaho Data Integration(Kettle).assets/image-20220224220710975.png)



底下为执行结果，可根据日志进行排查。

![image-20220224220922465](Pentaho Data Integration(Kettle).assets/image-20220224220922465.png)