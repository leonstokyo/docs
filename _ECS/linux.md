# Linux相关命令

#### **Firewall**相关

1. 查看所有打开的端口

`firewall-cmd --zone=public --list-ports`

2. 添加

`firewall-cmd --zone=public --add-port=80/tcp --permanent(--permanent永久生效，没有此参数重启后失效)`

3. 重新载入

`firewall-cmd --reload`

4. 查看

`firewall-cmd --zone=public --query-port=80/tcp`

5. 删除

`firewall-cmd --zone=public --remove-port=80/tcp --permanent`

6. 批量开放端口

`firewall-cmd --permanent --zone=public --add-port=100-500/tcp`
