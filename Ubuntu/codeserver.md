# codeserver
**2025/6/30**
起初打算使用WSL2+Ubuntu搭建我的codeserver,但不知为何,一旦我启动WSL2,我的电脑风扇就像直升机起飞一样聒噪.  
于是我转身看向了最经典同样也是最有名的虚拟机软件----VMware.
先从[官网](https://www.vmware.com/)下载VMware,然后安装.
再从[官网](https://cn.ubuntu.com/download)下载Ubuntu的ISO镜像,导入VMware.
由于Ubuntu安装完没有SSH,所以通过终端安装一个SSH  
```
sudo apt update
sudo apt install openssh-server
```
当然要设置开机自启  
```
sudo systemctl start ssh
sudo systemctl enable ssh
```
然后安装code server  
```
curl -fsSL https://code-server.dev/install.sh | sh
code-server
```
修改code server配置  
```  
nano ~/.config/code-server/config.yaml
```
大概长这样  
```  
bind-addr: 0.0.0.0:8080  # 允许所有IP访问
auth: password           # 启用密码验证
password: 你的密码        # 自定义访问密码（建议复杂密码）
cert: false              # 无HTTPS证书（局域网内可忽略）
```
修改完按ctrl+x保存并退出,然后重启codeserver
```
systemctl --user restart code-server
```
再通过SSH连接上Ubuntu,这一步要在宿主机里面运行  
```
ssh -L 8080:localhost:8080 <Ubuntu用户>@192.168.206.128
```
最后,浏览器访问 [http://localhost:8080](http://localhost:8080)，输入 code-server 密码即可。