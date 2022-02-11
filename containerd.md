**1.下载kata二进制并且安装**

https://github.com/kata-containers/kata-containers/releases/download/2.3.2/kata-static-2.3.2-x86_64.tar.xz

解压压缩包

xz -d kata-static-2.3.2-x86_64.tar.xz

tar xvf kata-static-2.3.2-x86_64.tar -C /

/opt/kata/bin下面把 containerd-shim-kata-v2和kata-runtime复制/usr/bin目录

```
[root@centos7 bin]# kata-runtime --version
kata-runtime  : 2.3.2
   commit   : 1af292c9e693e9bc8e8324a9eb860dad45306fb5
   OCI specs: 1.0.2-dev
```

**2.安装containerd和nerdctl**

tar Cxzvvf /usr/local nerdctl-full-0.16.0-linux-amd64.tar.gz

systemctl enable containerd

systemctl start containerd
