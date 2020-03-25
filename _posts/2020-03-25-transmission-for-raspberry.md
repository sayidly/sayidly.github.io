---
title: "树莓派安装 BitTorrent - Transmission"
published: true
---

- [rtorrent](https://github.com/rakshasa/rtorrent)
- [deluge](https://github.com/deluge-torrent/deluge)
- [transmission](https://github.com/transmission/transmission)

| name         | Headless | Size   | CLI | Stars |
|--------------|----------|--------|-----|-------|
| rtorrent     | yes      | 2.9MB | yes | 2.8k  |
| deluge       | yes      | 35.8MB  | yes | 0.8k  |
| transmission | yes      | 1MB   | yes | 3.9k  |

1. Check the size

```bash
$ apt-cache --no-all-versions show <package> | grep '^Size:'
```

2. References

- [Headless Torrect client that doesn't kill the Pi?](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=6084&sid=6d25bdf6b7238f52e16bdd55be44031b)
- [Recommendations for Headless Webserver](https://www.reddit.com/r/torrents/comments/6csctx/recommendations_for_headless_webserver/)
- [Torrent client for Raspberry Pi?](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=85174)

3. Comments

> The major downside of rTorrent is that its configuration is confusing as fuck, and there is almost no documentation. If you want to do anything fancy with it (like script it to do something interesting) I recommend you use deluge, which has a sane API, and good documentation.
> ruTorrent probably has the best web ui. Deluge is a close 2nd for web ui but it is really better to use with a thin client. Transmission works if you need something small without too many features, its whole goal is to be simple & straightforward.
> rTorrent is a command line ncurses BitTorrent client written in C++, based on the libTorrent. rTorrent is very lightweight and has a very clean command line user interface. rTorrent is also one of the most lightweight BitTorrent clients used by most of the BitTorrent distribution servers. rTorrent is a really good BitTorrent client for Raspberry Pi. rTorrent is lighter and faster compared to BitTorrent clients like Transmission, Deluge and takes lesser CPU resource.

4. Choose rTorrent(Update 3.25 hard to use)

- [RTORRENT ON RASPBERRY PI](https://darryldias.me/2015/rtorrent-raspberry-pi/)
- [Wiki](https://github.com/rakshasa/rtorrent/wiki)

Install
```bash
$ sudo apt-get update
$ sudo apt-get install screen rtorrent
```

Configure file
```bash
$ sudo vi ~/.rtorrent.rc
```

Or [using the template](https://github.com/rakshasa/rtorrent/wiki/CONFIG-Template)
```bash
$ curl -Ls "https://raw.githubusercontent.com/wiki/rakshasa/rtorrent/CONFIG-Template.md" \
    | sed -ne "/^######/,/^### END/p" \
    | sed -re "s:/home/USERNAME:$HOME:" >~/.rtorrent.rc
$ mkdir -p ~/rtorrent/
```

Now we can start using rTorrent.
```bash
$ screen rtorrent
```

You can detach to the rtorrent screen by typing CTRL + A + D.

To connect back to the rTorrent screen.

```bash
$ screen -r
```

| rTorrent keys    | Usage                                                                   |
|------------------|-------------------------------------------------------------------------|
| Cursor Up & Down | view torrents, peers, trackers etc                                      |
| Cursor Right     | display information on what has been navigated to.                      |
| Cursor Left      | Previous screen                                                         |
| Backspace        | Enter the URL or path of a new torrent to download.                     |
| Ctrl + q         | Exit rTorrent.                                                          |
| Ctrl + s         | Start a download / Resume the download                                  |
| Ctrl + d         | Stop a torrent / Stop download (A second time removes it from rTorrent) |
| Ctrl + k         | Close a torrent.                                                        |
| Ctrl + r         | Start hash checking a torrent.                                          |

## Transmission-daemon

Ref: [用树莓派搭建BT下载服务器](https://shumeipai.nxez.com/2013/09/08/raspberry-pi-bt-download-servers.html)

1. 首先安装 transmission：
```bash
$ sudo apt-get install transmission-daemon
```

2. 然后创建下载目录，一个是下载完成的目录，一个是未完成的目录，具体目录根据你的情况决定：
```bash
$ mkdir -p /home/pi/incomplete # for incomplete downloads
$ mkdir /home/pi/complete # finished downloads
```

3. 还要配置目录的权限：
```bash
$ sudo usermod -a -G debian-transmission pi
#如果是 fat 格式的移动硬盘无需下面这么改，mount的时候指定用户和读写权限就行
#这个是对 SD 卡上的目录而言的
$ sudo chgrp debian-transmission /home/pi/incomplete
$ sudo chgrp debian-transmission /home/pi/complete
$ sudo chmod 770 /home/pi/incomplete
$ sudo chmod 770 /home/pi/complete
```

4. 修改配置文件 /etc/transmission-daemon/settings.json ，这是一个 json 格式的文件，配置项很多，但重点改下面这些：

下载目录位置：
```
下载完成目录
"download-dir": "/home/pi/complete",
未完成的下载目录
"incomplete-dir": "/home/pi/incomplete",
启用未完成下载目录
"incomplate-dir-enable": true,
允许Web访问的白名单地址
"rpc-whitelist": "127.0.0.1,192.168.1.*",
```

5. 最后，配置好之后重启 transmission，注意以下两个命令按顺序执行，单独 restart 的话配置不会保存：

```bash
$ sudo service transmission-daemon reload
$ sudo service transmission-daemon restart
```
现在就好了，在浏览器中访问 IP 加 9091端口：比如： http://192.168.1.199:9091/ 。访问时输入用户名和密码，默认都是：transmission 。
你现在已经有了一个独立的 BT 下载服务器了！界面功能完备，可以做限速等设置。

修改 transmission 用户名和密码的方法：
1. 先停止服务： `sudo service transmission-daemon stop`
2. 修改配置文件，下面两项分别是用户和密码，你看到这个是加密的密码，没关系直接把密码改为你想要的密码明文就可以：
```
“rpc-username”: “transmission”,
“rpc-password”: “{2dc2c41724aab07ccc301e97f56360cb35f8ba1fGVVrdHDX”,
```
3. 再此启动服务 ：`sudo service transmission-daemon start` 。启动的时候 transmission 会自动把新密码加密。

### transmission 常用命令

停止
```bash
$ sudo /etc/init.d/transmission-daemon stop
$ sudo service transmission-daemon stop
```

重启配置
```bash
$ sudo service transmission-daemon reload
$ sudo service transmission-daemon restart
```

查看状态
```bash
$ sudo service transmission-daemon status
```

重置为默认
```bash
$ sudo systemctl stop transmission-daemon
$ sudo apt-get install --reinstall transmission-daemon
```

## 命令行启动 `transmission-remote`

参考：[TRANSMISSION CLI USER GUIDE](https://cli-ck.io/transmission-cli-user-guide/)

添加下载
```bash
$ transmission-remote -n 'transmission:transmission' -a "http://releases.ubuntu.com/16.10/ubuntu-16.10-desktop-amd64.iso.torrent"
```

下载状态
```bash
$ transmission-remote -n 'transmission:transmission' -l
```

```bash
$ alias tsm="transmission-remote"
$ tsm -l
```

删除下载 `-r`
```bash
$ transmission-remote -n 'transmission:transmission' -t <ID> -r
```

## 出现错误如 `transmission-daemon: UDP Failed to set receive / send buffer`

参考：[transmission-daemon: UDP Failed to set receive / send buffer](https://unix.stackexchange.com/questions/520625/transmission-daemon-udp-failed-to-set-receive-send-buffer)

处理如下：

1. 打开 `sysctl.conf` 文件
```bash
$ sudo vi /etc/sysctl.conf
```

2. 添加
```
net.core.rmem_max = 16777216
net.core.wmem_max = 4194304
```

3. 重新加载文件
```bash
$ sysctl -p
```

