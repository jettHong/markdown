# CentOS 7  安装 LibreOffice 7.2.6

0. 下载

   [下载 LibreOffice | LibreOffice 简体中文官方网站 - 自由免费的办公套件](https://zh-cn.libreoffice.org/download/libreoffice/)

   [国内镜像 7.2.6/rpm/x86_64/ (tencent.com)](https://mirrors.cloud.tencent.com/libreoffice/libreoffice/stable/7.2.6/rpm/x86_64/)

   LibreOffice_7.2.6_Linux_x86-64_rpm.tar.gz

   LibreOffice_7.2.6_Linux_x86-64_rpm_langpack_zh-CN.tar.gz

   LibreOffice_7.2.6_Linux_x86-64_rpm_helppack_zh-CN.tar.gz

1. 前置依赖

   * 程序运行依赖

   ```shell
   yum install -y cairo cups-libs libSM
   ```

   * 部分功能需要JDK环境

2. 安装

   分别按上面文件顺序解压，并进入相应 RPMS 目录进行安装。

   ```shell
   tar -xvf LibreOffice_7.2.6_Linux_x86-64_rpm.tar.gz
   tar -xvf LibreOffice_7.2.6_Linux_x86-64_rpm_langpack_zh-CN.tar.gz
   tar -xvf LibreOffice_7.2.6_Linux_x86-64_rpm_helppack_zh-CN.tar.gz
   
   # 进行本地安装
   yum localinstall -y LibreOffice_7.2.6.2_Linux_x86-64_rpm/RPMS/*.rpm
   yum localinstall -y LibreOffice_7.2.6.2_Linux_x86-64_rpm_langpack_zh-CN/RPMS/*.rpm
   yum localinstall -y LibreOffice_7.2.6.2_Linux_x86-64_rpm_helppack_zh-CN/RPMS/*.rpm
   ```

3. 验证

   ```shell
   # 安装成功验证
   libreoffice --version
   
   # 转换功能验证，将名为 test.doc 转换成 pdf
   libreoffice --headless --convert-to pdf test.doc
   ```

4. 字体

   [中日韩字体 | LibreOffice 简体中文官方网站 - 自由免费的办公套件](https://zh-cn.libreoffice.org/download/fonts/)

   ### 中日韩字体

   为了确保在不同操作环境下一致的显示效果，LibreOffice 内置了众多的西文字体，但是没有内置中日韩 (CJK) 字体，我们建议您自行下载下列中文字体。

   ## 思源黑体和思源宋体

   由 Adobe 和 Google 联合推出的开源 OpenType 字体思源黑体 (Source Han Sans) 和思源宋体 (Source Han Serif)，是我们推荐的用于 LibreOffice 的标准中日韩字体。其中，思源黑体适合用于标题样式以及屏幕显示，思源宋体适合用于正文文本以及打印显示。

   需要下载zip文件，解压缩后安装字体。其中，文件名中CN为简体中文字体，TW为繁体中文字体，KR为韩文字体，JP为日文字体。

   - GitHub 官方下载页面：[思源黑体，](https://github.com/adobe-fonts/source-han-sans/tree/release/SubsetOTF)[思源宋体](https://github.com/adobe-fonts/source-han-serif/tree/release/SubsetOTF)。
   - 清华大学开源镜像下载：[思源黑体，](https://mirrors.tuna.tsinghua.edu.cn/adobe-fonts/source-han-sans/SubsetOTF/)[思源宋体](https://mirrors.tuna.tsinghua.edu.cn/adobe-fonts/source-han-serif/SubsetOTF/)。

   ## 如何安装字体

   - Windows 下：请将下载后的字体文件解压缩，然后将文件复制到 %windir%\Fonts 文件夹。
   - Linux 下：将解压后的字体文件夹复制到 `/usr/share/fonts` 目录下，然后运行 `fc-cache -fv` 命令以更新字体缓存。

REF：

[linux安装libreOffice - smile_elims - 博客园 (cnblogs.com)](https://www.cnblogs.com/liangbo-/p/11424292.html)

[Centos7下安装Libreoffice并使用_向往的生活Life的博客-CSDN博客_centos7安装libreoffice](https://blog.csdn.net/ASAS1314/article/details/109028813)

[Linux 下的安装方法 | LibreOffice 简体中文官方网站 - 自由免费的办公套件](https://zh-cn.libreoffice.org/get-help/install-howto/linux/)

[中日韩字体 | LibreOffice 简体中文官方网站 - 自由免费的办公套件](https://zh-cn.libreoffice.org/download/fonts/)