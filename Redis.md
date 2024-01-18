# 一、Redis安装配置(WSL)

1. 在Windows系统中安装WSL

2. 在WSL中安装Redis
   
   ```shell
   sudo apt install redis-server
   ```

3. 配置Redis支持局域网访问
   
   ```shell
   # 修改/etc/redis/redis.conf文件
   sudo vim /etc/redis/redis.conf
   
   # 注释掉下面的配置
   # bind 127.0.0.1 ::1
   
   # 修改下面的配置为no
   protected-mode no
   ```
   
   ```shell
   # 启动Redis客户端
   redis-cli
   
   # 查看daemonize和protected-mode参数是否都为no？
   config get daemonize
   config get protected-mode
   
   # 如果上述参数不为no，则将其设置为no
   config set daemonize no
   config set protected-mode no
   ```

4. 重启Redis服务
   
   ```shell
   # 停止Redis服务
   sudo /etc/init.d/redis-server stop
   # 启动Redis服务
   sudo /etc/init.d/redis-server start
   # 重启Redis服务
   sudo /etc/init.d/redis-server restart
   ```

5. 在Windows上配置端口转发和防火墙允许入站规则（使用管理员权限打开PowerShell窗口）
   
   ```powershell
   # 查询WSL 2 IP地址（结果：172.28.14.0）
   wsl -- hostname -I
   
   # 配置端口转发
   netsh interface portproxy add v4tov4 listenport=6379 connectaddress=172.28.14.0 connectport=6379
   
   # 添加允许入站规则（注意：cmd窗口无法执行New-NetFirewallRule）
   New-NetFirewallRule -DisplayName "Allow Inbound TCP Port 6379" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 6379
   ```
   
   ```powershell
   # 删除上述配置命令
   
   # 删除端口转发规则
   netsh interface portproxy delete v4tov4 listenport=6379
   
   # 删除防火墙入站规则
   Remove-NetFirewallRule -DisplayName "Allow Inbound TCP Port 6379"
   ```

6. WSL启动Redis报错（Could not create Server TCP listening socket *:6379: bind: Address already in use）解决方法
   
   ```shell
   # 找到Redis进程
   ps -ef | grep redis
   root    3086      1    0 Apr24  ?       00:00:07  ./bin/redis-server  *:6379
   root    3531   3467    0 01:00  pts/0   00:00:00  grep -i redis
   
   # 通过进程号杀死该进程
   kill 3086
   
   # 重新启动Redis即可
   redis-server
   ```
