**1.以docker 17.12为例** 
参考：(https://blog.csdn.net/sjy8207380/article/details/85125531)

**一些概念**

**ImageID：唯一标志一个镜像，其数值根据该镜像的元数据配置文件采用sha256算法的计算获得**

**cacheID:由宿主机随即生成的一个uuid，根镜像层文件一一对应，用于宿主机标志和索引镜像层文件**

**diffID:镜像层校验ID、根据该镜像层的打包文件校验获得**

**chainID:docker内容寻址机制采用的索引ID，其值根据当前层和所有祖先层的diffID算得**：

- 若该镜像层是最底层，那么其chainID 和 diffID 相同
- 否则，chainID=sha256(父层chainID+" "+本层diffID)

**parent：父镜像层的chainID(最底层不含该文件)**

cat /etc/docker/daemon.json

```
{
    "storage-driver": "overlay2",
    "storage-opts": [
        "overlay2.override_kernel_check=true"
    ]
}
```

**2.docker pull nginx:1.21**

docker 默认采用 SHA256 算法根据镜像元数据配置文件计算出镜像 ID。两条记录本质上是一样的，第二条记录和第一条记录指向同一个镜像 ID。其中"sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85 被称为镜像的摘要

- 镜像摘要Digest：SHA256对镜像manifest内容计算
- 对于本地生成的镜像来说，由于没有上传到registry上去，所以没有digest，因为镜像的manifest由registry生成

```
[root@centos7 overlay2]# cat /var/lib/docker/image/overlay2/repositories.json |python -m json.tool
{
 "Repositories": {
 "nginx": {
 "nginx:1.21": "sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85" （镜像摘要）
 }
 }
}
```

**3.查看镜像config**

**/var/lib/docker/image/overlay2/imagedb/content/sha256/605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85 (镜像id)**

```
[root@centos7 sha256]# cat /var/lib/docker/image/overlay2/imagedb/content/sha256/605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85 |python -m json.tool
{
    "architecture": "amd64",
    "config": {
        "AttachStderr": false,
        "AttachStdin": false,
        "AttachStdout": false,
        "Cmd": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "Domainname": "",
        "Entrypoint": [
            "/docker-entrypoint.sh"
        ],
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
            "NGINX_VERSION=1.21.5",
            "NJS_VERSION=0.7.1",
            "PKG_RELEASE=1~bullseye"
        ],
        "ExposedPorts": {
            "80/tcp": {}
        },
        "Hostname": "",
        "Image": "sha256:82941edee2f4d17c55563bb926387c3ae39fa1a99777f088bc9d3db885192209",
        "Labels": {
            "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
        },
        "OnBuild": null,
        "OpenStdin": false,
        "StdinOnce": false,
        "StopSignal": "SIGQUIT",
        "Tty": false,
        "User": "",
        "Volumes": null,
        "WorkingDir": ""
    },
    "container": "ca3e48389f7160bc9d9a892d316fcbba459344ee3679998739b1c3cd8e56f7da",
    "container_config": {
        "AttachStderr": false,
        "AttachStdin": false,
        "AttachStdout": false,
        "Cmd": [
            "/bin/sh",
            "-c",
            "#(nop) ",
            "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
        ],
        "Domainname": "",
        "Entrypoint": [
            "/docker-entrypoint.sh"
        ],
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
            "NGINX_VERSION=1.21.5",
            "NJS_VERSION=0.7.1",
            "PKG_RELEASE=1~bullseye"
        ],
        "ExposedPorts": {
            "80/tcp": {}
        },
        "Hostname": "ca3e48389f71",
        "Image": "sha256:82941edee2f4d17c55563bb926387c3ae39fa1a99777f088bc9d3db885192209",
        "Labels": {
            "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
        },
        "OnBuild": null,
        "OpenStdin": false,
        "StdinOnce": false,
        "StopSignal": "SIGQUIT",
        "Tty": false,
        "User": "",
        "Volumes": null,
        "WorkingDir": ""
    },
    "created": "2021-12-29T19:28:29.892199479Z",
    "docker_version": "20.10.7",
    "history": [
        {
            "created": "2021-12-21T01:22:43.418913408Z",
            "created_by": "/bin/sh -c #(nop) ADD file:09675d11695f65c55efdc393ff0cd32f30194cd7d0fbef4631eebfed4414ac97 in / "
        },
        {
            "created": "2021-12-21T01:22:43.799429634Z",
            "created_by": "/bin/sh -c #(nop)  CMD [\"bash\"]",
            "empty_layer": true
        },
        {
            "created": "2021-12-21T03:00:06.15011478Z",
            "created_by": "/bin/sh -c #(nop)  LABEL maintainer=NGINX Docker Maintainers <docker-maint@nginx.com>",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:08.071260391Z",
            "created_by": "/bin/sh -c #(nop)  ENV NGINX_VERSION=1.21.5",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:08.248669935Z",
            "created_by": "/bin/sh -c #(nop)  ENV NJS_VERSION=0.7.1",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:08.472939138Z",
            "created_by": "/bin/sh -c #(nop)  ENV PKG_RELEASE=1~bullseye",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:27.976946316Z",
            "created_by": "/bin/sh -c set -x     && addgroup --system --gid 101 nginx     && adduser --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos \"nginx user\" --shell /bin/false --uid 101 nginx     && apt-get update     && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates     &&     NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62;     found='';     for server in         hkp://keyserver.ubuntu.com:80         pgp.mit.edu     ; do         echo \"Fetching GPG key $NGINX_GPGKEY from $server\";         apt-key adv --keyserver \"$server\" --keyserver-options timeout=10 --recv-keys \"$NGINX_GPGKEY\" && found=yes && break;     done;     test -z \"$found\" && echo >&2 \"error: failed to fetch GPG key $NGINX_GPGKEY\" && exit 1;     apt-get remove --purge --auto-remove -y gnupg1 && rm -rf /var/lib/apt/lists/*     && dpkgArch=\"$(dpkg --print-architecture)\"     && nginxPackages=\"         nginx=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-xslt=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-geoip=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-image-filter=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-njs=${NGINX_VERSION}+${NJS_VERSION}-${PKG_RELEASE}     \"     && case \"$dpkgArch\" in         amd64|arm64)             echo \"deb https://nginx.org/packages/mainline/debian/ bullseye nginx\" >> /etc/apt/sources.list.d/nginx.list             && apt-get update             ;;         *)             echo \"deb-src https://nginx.org/packages/mainline/debian/ bullseye nginx\" >> /etc/apt/sources.list.d/nginx.list                         && tempDir=\"$(mktemp -d)\"             && chmod 777 \"$tempDir\"                         && savedAptMark=\"$(apt-mark showmanual)\"                         && apt-get update             && apt-get build-dep -y $nginxPackages             && (                 cd \"$tempDir\"                 && DEB_BUILD_OPTIONS=\"nocheck parallel=$(nproc)\"                     apt-get source --compile $nginxPackages             )                         && apt-mark showmanual | xargs apt-mark auto > /dev/null             && { [ -z \"$savedAptMark\" ] || apt-mark manual $savedAptMark; }                         && ls -lAFh \"$tempDir\"             && ( cd \"$tempDir\" && dpkg-scanpackages . > Packages )             && grep '^Package: ' \"$tempDir/Packages\"             && echo \"deb [ trusted=yes ] file://$tempDir ./\" > /etc/apt/sources.list.d/temp.list             && apt-get -o Acquire::GzipIndexes=false update             ;;     esac         && apt-get install --no-install-recommends --no-install-suggests -y                         $nginxPackages                         gettext-base                         curl     && apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/nginx.list         && if [ -n \"$tempDir\" ]; then         apt-get purge -y --auto-remove         && rm -rf \"$tempDir\" /etc/apt/sources.list.d/temp.list;     fi     && ln -sf /dev/stdout /var/log/nginx/access.log     && ln -sf /dev/stderr /var/log/nginx/error.log     && mkdir /docker-entrypoint.d"
        },
        {
            "created": "2021-12-29T19:28:28.390440509Z",
            "created_by": "/bin/sh -c #(nop) COPY file:65504f71f5855ca017fb64d502ce873a31b2e0decd75297a8fb0a287f97acf92 in / "
        },
        {
            "created": "2021-12-29T19:28:28.640632824Z",
            "created_by": "/bin/sh -c #(nop) COPY file:0b866ff3fc1ef5b03c4e6c8c513ae014f691fb05d530257dfffd07035c1b75da in /docker-entrypoint.d "
        },
        {
            "created": "2021-12-29T19:28:28.864872406Z",
            "created_by": "/bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7de297435e32af634f29f7132ed0550d342cad9fd20158258 in /docker-entrypoint.d "
        },
        {
            "created": "2021-12-29T19:28:29.113627588Z",
            "created_by": "/bin/sh -c #(nop) COPY file:09a214a3e07c919af2fb2d7c749ccbc446b8c10eb217366e5a65640ee9edcc25 in /docker-entrypoint.d "
        },
        {
            "created": "2021-12-29T19:28:29.296888541Z",
            "created_by": "/bin/sh -c #(nop)  ENTRYPOINT [\"/docker-entrypoint.sh\"]",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:29.498026198Z",
            "created_by": "/bin/sh -c #(nop)  EXPOSE 80",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:29.705311925Z",
            "created_by": "/bin/sh -c #(nop)  STOPSIGNAL SIGQUIT",
            "empty_layer": true
        },
        {
            "created": "2021-12-29T19:28:29.892199479Z",
            "created_by": "/bin/sh -c #(nop)  CMD [\"nginx\" \"-g\" \"daemon off;\"]",
            "empty_layer": true
        }
    ],
    "os": "linux",
    "rootfs": {
        "diff_ids": [
          (0)  "sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f",
          (1) "sha256:e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8",
          (2)  "sha256:b8d6e692a25e11b0d32c5c3dd544b71b1085ddc1fddad08e68cbd7fda7f70221",
            "sha256:f1db227348d0a5e0b99b15a096d930d1a69db7474a1847acbc31f05e4ef8df8c",
            "sha256:32ce5f6a5106cc637d09a98289782edf47c32cb082dc475dd47cbf19a4f866da",
            "sha256:d874fd2bc83bb3322b566df739681fbd2248c58d3369cb25908d68e7ed6040a6"
        ],
        "type": "layers"
    }
}
```

**4.layer元数据**

**/var/lib/docker/image/overlay2/layerdb/sha256保留layer元数据****

**每个diffid对应一个layer tar包的sha256**

layer包含了上一个layer上的改动情况，主要包含三方面的内容：
1）变化类型：是增加、修改还是删除了文件
2）文件类型：每个变化发生在哪种文件类型上
3）文件属性：文件的修改时间、用户ID、组ID、RWX权限等
chainID的计算方法
1）如果该镜像层是最底层(没有父镜像层)，该层的 diffID 便是 chainID。
2）该镜像层的 chainID 计算公式为 chainID(n)=SHA256(chain(n-1) diffID(n))，也就是根据父镜像层的 chainID 加上一个空格和当前层的 diffID，再计算 SHA256 校验码。

```
目录/var/lib/docker/image/overlay2/layerdb/sha256
[root@centos7 sha256]# tree
.
├── 02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4
│   ├── cache-id
│   ├── diff
│   ├── parent
│   ├── size
│   └── tar-split.json.gz
├── 2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f
│   ├── cache-id
│   ├── diff
│   ├── size
│   └── tar-split.json.gz
├── 780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5
│   ├── cache-id
│   ├── diff
│   ├── parent
│   ├── size
│   └── tar-split.json.gz
├── 7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a14
│   ├── cache-id
│   ├── diff
│   ├── parent
│   ├── size
│   └── tar-split.json.gz
├── b625d8e29573fa369e799ca7c5df8b7a902126d2b7cbeb390af59e4b9e1210c5
│   ├── cache-id
│   ├── diff
│   ├── parent
│   ├── size
│   └── tar-split.json.gz
└── b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c
    ├── cache-id
    ├── diff
    ├── parent
    ├── size
    └── tar-split.json.gz

6 directories, 29 files
```

- chche-id: 本机随机生成的uuid用来标识在本机器上唯一
- diff: diffId
- parent: 父layer的chainId
- size: 这个layer的大小

**最底层：chainID=diffID**

**其他层公式：chainID(n)=sha256sum(chainID(n-1)) diffID(n))**

**chainID(0)=sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f**
chainID(1) = echo -n 'sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f sha256:e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8' | sha256sum
结果是**chainID(1)=sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5**

**chaiID(2)=sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c **

**chainID(3)==sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4**

**chainID(4)=sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a14**

**chainID(5)=sha256:b625d8e29573fa369e799ca7c5df8b7a902126d2b7cbeb390af59e4b9e1210c5**

**目录/var/lib/docker下**

```
[root@centos7 docker]# tree -L 4
.
├── builder
│   └── fscache.db
├── containerd
│   └── daemon
│       ├── io.containerd.content.v1.content
│       │   └── ingest
│       ├── io.containerd.metadata.v1.bolt
│       │   └── meta.db
│       ├── io.containerd.runtime.v1.linux
│       ├── io.containerd.snapshotter.v1.btrfs
│       └── io.containerd.snapshotter.v1.overlayfs
│           └── snapshots
├── containers
├── image
│   └── overlay2
│       ├── distribution
│       │   ├── diffid-by-digest
│       │   └── v2metadata-by-diffid
│       ├── imagedb
│       │   ├── content
│       │   └── metadata
│       ├── layerdb
│       │   ├── sha256
│       │   └── tmp
│       └── repositories.json
├── network
│   └── files
│       └── local-kv.db
├── overlay2
│   ├── 371e2f28c55cf364a63c147363e6c63cd7aa33759413bf82d07d0b02763bda35
│   │   ├── diff
│   │   │   └── docker-entrypoint.d
│   │   ├── link
│   │   ├── lower
│   │   └── work
│   ├── 40dcc790079b2fd0fb02d641bd365b93b9da909af19e007806a1c7877e46a26c
│   │   ├── diff
│   │   │   ├── anaconda-post.log
│   │   │   ├── bin -> usr/bin
│   │   │   ├── dev
│   │   │   ├── etc
│   │   │   ├── home
│   │   │   ├── lib -> usr/lib
│   │   │   ├── lib64 -> usr/lib64
│   │   │   ├── media
│   │   │   ├── mnt
│   │   │   ├── opt
│   │   │   ├── proc
│   │   │   ├── root
│   │   │   ├── run
│   │   │   ├── sbin -> usr/sbin
│   │   │   ├── srv
│   │   │   ├── sys
│   │   │   ├── tmp
│   │   │   ├── usr
│   │   │   └── var
│   │   └── link
│   ├── 6040edc080d9c2372f83cba63ef2cacff0492b612fdcc453fc8578ade8c20e56
│   │   ├── diff
│   │   │   └── docker-entrypoint.sh
│   │   ├── link
│   │   ├── lower
│   │   └── work
│   ├── 8069c68810e7ab689858c7b7d1d9ca3ef4b9b69b6fb5288241b35f9dac2590c6
│   │   ├── diff
│   │   │   ├── docker-entrypoint.d
│   │   │   ├── etc
│   │   │   ├── lib
│   │   │   ├── tmp
│   │   │   ├── usr
│   │   │   └── var
│   │   ├── link
│   │   ├── lower
│   │   └── work
│   ├── a1c4d12260507189637103755dcf250e208522b52acfc7c0d533a76a33270c71
│   │   ├── diff
│   │   │   └── docker-entrypoint.d
│   │   ├── link
│   │   ├── lower
│   │   └── work
│   ├── b6a96c0209c8ab8919dc620fa12be087d2a6398b9e839c17f195ced771ea2e25
│   │   ├── diff
│   │   │   └── docker-entrypoint.d
│   │   ├── link
│   │   ├── lower
│   │   └── work
│   ├── backingFsBlockDev
│   ├── f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08
│   │   ├── diff
│   │   │   ├── bin
│   │   │   ├── boot
│   │   │   ├── dev
│   │   │   ├── etc
│   │   │   ├── home
│   │   │   ├── lib
│   │   │   ├── lib64
│   │   │   ├── media
│   │   │   ├── mnt
│   │   │   ├── opt
│   │   │   ├── proc
│   │   │   ├── root
│   │   │   ├── run
│   │   │   ├── sbin
│   │   │   ├── srv
│   │   │   ├── sys
│   │   │   ├── tmp
│   │   │   ├── usr
│   │   │   └── var
│   │   └── link
│   └── l
│       ├── 3BHZGQ3WTNUD72A6ASYNWJF6QW -> ../8069c68810e7ab689858c7b7d1d9ca3ef4b9b69b6fb5288241b35f9dac2590c6/diff
│       ├── 4BARNU5OAAOAWLVGD5COE25URN -> ../a1c4d12260507189637103755dcf250e208522b52acfc7c0d533a76a33270c71/diff
│       ├── DYQPFGRYDDVV65U4LVONVOUCK4 -> ../40dcc790079b2fd0fb02d641bd365b93b9da909af19e007806a1c7877e46a26c/diff
│       ├── GGGEPU5TSLFN5G67PUVHP2ZX6R -> ../371e2f28c55cf364a63c147363e6c63cd7aa33759413bf82d07d0b02763bda35/diff
│       ├── J2A2FWM7UTF3KXP6FTXYBNUPL2 -> ../b6a96c0209c8ab8919dc620fa12be087d2a6398b9e839c17f195ced771ea2e25/diff
│       ├── OWU5EFNXLYOCWTFPRLMYWV7Q4I -> ../6040edc080d9c2372f83cba63ef2cacff0492b612fdcc453fc8578ade8c20e56/diff
│       └── XJQTWATTEH53NRDZTYRSXEOJR4 -> ../f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08/diff
├── plugins
│   ├── storage
│   │   └── blobs
│   │       └── tmp
│   └── tmp
├── runtimes
├── swarm
├── tmp
├── trust
└── volumes
    └── metadata.db
```

**cd /var/lib/docker/overlay2/f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08查看大小**

```
[root@centos7 f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08]# du -s
87924=85.8m     .


[root@centos7 2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f]# cat /var/lib/docker/image/overlay2/layerdb/sha256/2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f/size
80372237=76.6m
```

**cat cache-id实际位置**

f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08

**cd** **/var/lib/docker/overlay2/f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08进行恢复镜像层 其中tar-split.json.gz取自 /var/lib/docker/image/overlay2/layerdb/sha256/2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f**

```
├── 2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f
│   ├── cache-id
│   ├── diff
│   ├── size
│   └── tar-split.json.gz
```

**恢复镜像层压缩文件(diff目录)**

```
[root@centos7 f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08]#  tar-split asm --output new.tar --input ./tar-split.json.gz  --path ./diff/
INFO[0013] created new.tar from ./diff/ and ./tar-split.json.gz (wrote 83858432 bytes)

[root@centos7 f2189063eec9479a58c43c8e884e37e86ef239ee0745199f9ebea62ebc298f08]#  sha256sum new.tar
2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f  new.tar
```

**5.校验hash值 sha256sum new.tar正确是diffID(0)表明镜像层是完整的没有缺失**

2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f

同理校验下一层

/var/lib/docker/overlay2/8069c68810e7ab689858c7b7d1d9ca3ef4b9b69b6fb5288241b35f9dac2590c6

下tar-split asm --output new.tar --input ./tar-split.json.gz --path ./diff/ 生成new.tar

```
sha256sum new.tar
e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8  new.tar
```

**e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8 = diffID(1)**
