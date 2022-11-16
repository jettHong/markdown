# 数据抽取-Spoon

### 一、背景

当前项目有企业数据抽取的需求，有各种页面数据汇总的需求（实际为各种数据的处理，转换，迁移），对于这类需求业内已有优秀的实现，并将这类过程工具称为ETL（Extract-Transform-Load的缩写，即数据抽取、转换、装载的过程）工具。

### 二、Spoon工具的介绍

Spoon 支持图形化的[GUI](https://so.csdn.net/so/search?q=GUI&spm=1001.2101.3001.7020)设计界面，然后可以以工作流的形式流转，在做一些简单或复杂的数据抽取、质量检测、数据清洗、数据转换、数据过滤等方面有着比较稳定的表现，使用它减少了非常多的研发工作量，提高了我们的工作效率。

### 三、使用文档

http://svn.syncsoft.com:3699/svn/ProjectSVN/108_产业扶持产品化建设/02开发区/02设计/06库表设计/spoon/Windows-spoon操作手册.docx

http://svn.syncsoft.com:3699/svn/ProjectSVN/108_产业扶持产品化建设/02开发区/02设计/06库表设计/spoon/Linux部署Spoon(Kettle)手册(附作业部署、定时任务配置).docx

### 四、其它要求

一般将程序目录及脚本目录分享存放。

程序路径：/usr/local/data-integration
脚本路径：/usr/local/spoonTask
定时任务配置：crontab -e
数据库信息：/usr/local/spoonTask/jdbc_config.properties

部署生产环境需需要回写相应配置信息，例如：http://svn.syncsoft.com:3699/svn/ProjectSVN/309_荆州市惠企政策直达服务平台/06实施运维区/02实施文档/01网络拓扑图/荆州惠企云-服务器信息（包含应用）.xlsx