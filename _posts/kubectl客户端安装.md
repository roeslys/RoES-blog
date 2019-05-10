## 在Linux上安装kubectl

### 在Linux上用curl安装kubectl二进制文件

1. 使用以下命令下载最新版本：

   ```
   curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
   ```

   要下载特定版本，请将`$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)`命令部分替换为特定版本。

   例如，要在Linux上下载v1.14.0版，请键入：

   ```
   curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.14.0/bin/linux/amd64/kubectl
   ```

   结果发现被墙了，服务端没有翻墙工具，就在本机下载后上传到服务器上了。

2. 制作kubectl二进制可执行文件。

   ```
   chmod +x ./kubectl
   ```

3. 将二进制文件移动到PATH中。

   ```
   sudo mv ./kubectl /usr/local/bin/kubectl
   ```

4. 测试以确保您安装的版本是最新的：

   ```
   kubectl version
   ```

