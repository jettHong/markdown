# Jar文件的替换修改

```sh
# 直接修改 jar 内文件
vim biz.jar
#搜索文件 /文件名 或者 ?文件名
/文件名
# 回车进入要修改的文件，按 i 进入编辑模式。
```



```sh
# 1、复制到一个新目录去操作
mk renew
cp cyfc.jar renew
cd renew

# 2、解压
tar -xvf cyfc.jar
rm cyfc.jar -f

# 3、 替换新的业务jar到renew/BOOT-INF/lib 或者使用上面的直接修改方法

# 4、还原
jar -cfM0 cyfc.jar ./

```

```
jar
用法: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...
选项:
    -c  创建新档案
    -t  列出档案目录
    -x  从档案中提取指定的 (或所有) 文件
    -u  更新现有档案
    -v  在标准输出中生成详细输出
    -f  指定档案文件名
    -m  包含指定清单文件中的清单信息
    -n  创建新档案后执行 Pack200 规范化
    -e  为捆绑到可执行 jar 文件的独立应用程序
        指定应用程序入口点
    -0  仅存储; 不使用任何 ZIP 压缩
    -P  保留文件名中的前导 '/' (绝对路径) 和 ".." (父目录) 组件
    -M  不创建条目的清单文件
    -i  为指定的 jar 文件生成索引信息
    -C  更改为指定的目录并包含以下文件
如果任何文件为目录, 则对其进行递归处理。
清单文件名, 档案文件名和入口点名称的指定顺序
与 'm', 'f' 和 'e' 标记的指定顺序相同。

示例 1: 将两个类文件归档到一个名为 classes.jar 的档案中:
       jar cvf classes.jar Foo.class Bar.class
示例 2: 使用现有的清单文件 'mymanifest' 并
           将 foo/ 目录中的所有文件归档到 'classes.jar' 中:
       jar cvfm classes.jar mymanifest -C foo/ .
```

REF：https://blog.csdn.net/weixin_43908525/article/details/108317009

