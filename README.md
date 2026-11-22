# 运维转行 · 第一周实践记录

> 环境：VirtualBox + Ubuntu 26.04 LTS Server（无桌面）｜Windows 宿主机｜用户名 lzl
> 第一周 7 天，每天 ≤2 小时，把「装环境 → 连得上 → 服务跑起来 → 修得回来」走一遍。

## 第 1 天：装 Linux，只用命令行
- 装好 Ubuntu Server，开机就是黑底终端
- 建用户、改权限：chmod 600 后变成 -rw-------
- 看进程、看磁盘：free -h、df -h

## 第 2 天：把命令记进文档
- 本次跳过，待补（第 7 天重做时一起整理）

## 第 3 天：配好网络，本机 ping 通、ssh 连得上
- 桥接不通 → 改用 NAT + 端口转发 2222 → 22
- hostname -I 查 IP；Windows 上 ping -n 4 <IP> 通了
- sudo systemctl enable --now ssh，22 端口有 LISTEN
- 连接：ssh -p 2222 lzl@127.0.0.1
- 看懂的报错：Connection refused（端口没人听）、Connection reset（被对方掐断）、Permission denied（认证没过）

## 第 4 天：装 nginx，浏览器能打开默认页
- sudo apt install -y nginx；systemctl status nginx 看到 active (running)
- NAT 再加一条转发 8080 → 80
- 浏览器打开 http://127.0.0.1:8080 → Welcome to nginx!
- 80 是 nginx 守着的端口，8080 是 Windows 这边的门，127.0.0.1 指本机

## 第 5 天：故意改错配置把 nginx 弄坏，再看日志修回来（：sudo cp / nginx -t → syntax is ok / test is successful
3. 故意写错：往配置末尾追加一行 THIS_LINE_IS_WRONG;
4. 再体检：unknown directive "THIS_LINE_IS_WRONG" in /etc/nginx/nginx.conf:35 → 文件、行号、原因
5. 重启：sudo systemctl restart nginx → Job for nginx.service failed
6. 看日志：sudo journalctl -u nginx -n 15 --no-pager → 一条因果链「不认识的词 → 自检没过 → 启动失败」
7. 用备份恢复：sudo cp /etc/nginx/nginx.conf.bak /etc/nginx/nginx.conf
8. 复核：sudo nginx -t 重新通过
9. 重启 + 刷新浏览器 → Welcome to nginx! 回来

> 我明白了：报错不是麻烦，是线索——它会写明哪个文件、第几行错了。
