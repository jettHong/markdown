# CentOS8 安装 Font

1. 创建单独目录已存放中文字体

   ```sh
   mkdir /usr/share/fonts/zh -p
   ```

   

2. 上传中文字体

   复制 `C:\Windows\Fonts` 目录下：**楷体**,**宋体**，**黑体**，**仿宋**，**微软雅黑**，**Times New Roman** 5 类字体到步骤1目录中

   

3. 刷新字体

   ```sh
   fc-cache /usr/share/fonts/zh/
   ```

   

4. 检查

   ```sh
   # 检查字体是否加载
   fc-list
   ```

   

5. EOF

