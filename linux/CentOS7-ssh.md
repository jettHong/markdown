# ssh 免密登录

0. ssh是啥

1. 本地系统生成密钥文件

   ```sh
   ssh-keygen -t rsa
   # 输入文件存储位置
   # Generating public/private rsa key pair.
   # Enter file in which to save the key (/root/.ssh/id_rsa):
   # Created directory '/root/.ssh'.
   # 输入主密钥
   # Enter passphrase (empty for no passphrase):
   # Enter same passphrase again:
   # Your identification has been saved in /root/.ssh/id_rsa.
   # Your public key has been saved in /root/.ssh/id_rsa.pub.
   # 直接多次回车即可
   ```

2. 复制本地公共密钥到目标服务中

   ```sh
   ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.81.202
   # Are you sure you want to continue connecting (yes/no)? yes
   # root@192.168.81.202's password:: 输入目标服务器密码
   ```

3. 验证

   ```sh
   ssh root@192.168.81.202
   # 直接登陆，不再提示要求输入密码
   ```

4. EOF