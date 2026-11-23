# 常用命令速查

> 第二周整理：从第一周 README 里挑出命令，补上「什么时候用」和「输出看哪行」。

| 命令 | 什么时候用 | 输出看哪行 |
| --- | --- | --- |
| chmod 600 | 修改文件所有者、所属组、其他的读写执行权限；用 ls -l 看三组 rwx 是否变化，变了就说明修改成功 | ls -l 看到 -rw------- |
| df -h | 查看磁盘空间 | Use% 那一列 |
| sudo apt install -y nginx | 安装 nginx 服务 | 最后出现 Setting up nginx，回到提示符 |
| systemctl status nginx | 查看 nginx 服务的状态，看到 active(running) 或者 active(dead) 就算查到了 | Active: 那一行 |
| sudo nginx -t | 检查 nginx 配置文件是否正常，看到 is ok 就是正常的 | syntax is ok / test is successful |
| sudo systemctl restart nginx | 重启 nginx 服务，看到前后 Main PID 不一样就是成功 | 什么都不打印，直接回提示符 |
