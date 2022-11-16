# ETL

> **ETL**，是英文Extract-Transform-Load的缩写，用来描述将[数据](https://baike.baidu.com/item/数据/5947370?fromModule=lemma_inlink)从来源端经过抽取（extract）、[转换](https://baike.baidu.com/item/转换/197560?fromModule=lemma_inlink)（transform）、加载（load）至目的端的过程。**ETL**一词较常用在[数据仓库](https://baike.baidu.com/item/数据仓库?fromModule=lemma_inlink)，但其对象并不限于数据仓库。

## **ETL工具综合对比:**

| 序号 | 国籍             | 名称                                                         | 价格           |
| ---- | ---------------- | ------------------------------------------------------------ | -------------- |
| 1    | 美国(数据库自带) | ODI                                                          | 数据库厂商提供 |
| 2    | 美国(1993年创立) | informatica                                                  | 70-100w        |
| 3    | 中国(2014年创立) | Taskctl                                                      | 免费-50w       |
| 4    | 美国(2005年收购) | [datastage](https://www.zhihu.com/search?q=datastage&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"311797775"}) | 40-80w         |
| 5    | 英国(2006年开源) | kettle                                                       | 免费           |

**评价:**

- **ODI**有局限性，与 oracle 数据库耦合太深，做不到异构数据库之间(C/S架构）;[下载获取](https://link.zhihu.com/?target=https%3A//www.oracle.com/cn/middleware/technologies/data-integrator/downloads.html)
- **Informatica**是业界最专业ETL工具。但是对CPU和内存、硬盘的要求过高,需慎选（C/S架构）;[下载获取](https://link.zhihu.com/?target=http%3A//pan.baidu.com/share/link%3Fshareid%3D183201%26uk%3D67437475)
- **Taskctl**是国内最早ETL工具产品商。支持10万级作业规模调度,国人核心技术，一直紧追国际专业产品（C/S架构）;[下载获取](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/wBRLNmy2c52Dr6xuwgzKNw)
- **Datastage**与[informatica](https://www.zhihu.com/search?q=informatica&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"311797775"})旗鼓相当。没有断点恢复能力；自身调度、监控客户端功能较薄弱（C/S架构）;[下载获取](https://link.zhihu.com/?target=ftp%3A//ftp.seu.edu.cn/Pub/Develop)
- **Kettle**是业界最有名的开源ETL工具。首先数据安全隐患；其次服务跟进问题。需要借助第三方调度工具控制作业执行时间、稳定性差，需要定期重启软件；缺乏[元数据管理](https://www.zhihu.com/search?q=元数据管理&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"311797775"})；后期维护管理成本不可预估......闭源是个地雷！[下载获取](https://link.zhihu.com/?target=http%3A//kettle.pentaho.com/)



## Kettle

操作系统级别的调度器：对于ETL来说，调度不是独一无二的。这是操作系统能够提供标准调度的一般性需要，如UNIX衍生系统上的Cron以及Windows系统上的任务调度器。这些调度器能够拥戴调度Kettle命令行程序来运行任务和转换。