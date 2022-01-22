**1. nerdctl  pull nginx:1.21**

```
[root@centos7 ~]# nerdctl pull  nginx:1.21
docker.io/library/nginx:1.21:                                                     resolved       |++++++++++++++++++++++++++++++++++++++|
index-sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31:    done           |++++++++++++++++++++++++++++++++++++++|
manifest-sha256:ee89b00528ff4f02f2405e4ee221743ebc3f8e8dd0bfd5c4c20a2fa2aaa7ede3: done           |++++++++++++++++++++++++++++++++++++++|
config-sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85:   done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:589b7251471a3d5fe4daccdddfefa02bdc32ffcba0a6d6a2768bf2c401faf115:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:a0bcbecc962ed2552e817f45127ffb3d14be31642ef3548997f58ae054deb5b2:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:a2abf6c4d29d43a4bf9fbb769f524d0fb36a2edab49819c1bf3e76f409f953ea:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:a9edb18cadd1336142d6567ebee31be2a03c0905eeefe26cb150de7b0fbc520b:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:b4df32aa5a72e2a4316aad3414508ccd907d87b4ad177abd7cbd62fa4dab2a2f:    done           |++++++++++++++++++++++++++++++++++++++|
elapsed: 30.1s                                                                    total:  12.8 K (434.0 B/s)
```

**/var/lib/containerd/io.containerd.metadata.v1.bolt目录下查看**

**boltbrowser 1meta.db （meta.db的复制）**

```
                                                                                     boltbrowser: 1meta.db                                                                                    
===============================================================================================|==============================================================================================
  - v1                                                                                         | Path: v1 → default → content → ingests                                                       
    - default                                                                                  | Buckets: 0                                                                                   
      - content                                                                                | Pairs: 0                                                                                     
        - blob                                                                                                                                                    |                                                                                              
          + sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31(index-sha256)                                                                                           + sha256:186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d            |                                                                                
          + sha256:589b7251471a3d5fe4daccdddfefa02bdc32ffcba0a6d6a2768bf2c401faf115            |                                                                                              
          + sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85 (config-sha256)                                                                                            
          + sha256:a0bcbecc962ed2552e817f45127ffb3d14be31642ef3548997f58ae054deb5b2            |                                                                                              
          + sha256:a2abf6c4d29d43a4bf9fbb769f524d0fb36a2edab49819c1bf3e76f409f953ea            |                                                                                              
          + sha256:a9edb18cadd1336142d6567ebee31be2a03c0905eeefe26cb150de7b0fbc520b            |                                                                                              
          + sha256:b4df32aa5a72e2a4316aad3414508ccd907d87b4ad177abd7cbd62fa4dab2a2f            |                                                                                              
          + sha256:ee89b00528ff4f02f2405e4ee221743ebc3f8e8dd0bfd5c4c20a2fa2aaa7ede3 (manifest-sha256)                                                                                                   
        - ingests                                                                              |                                                                                              
      - images                                                                                 |                                                                                              
        - docker.io/library/nginx:1.21                                                         |                                                                                              
          - target                                                                             |                                                                                              
            digest: sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31    |                                                                                              
            mediatype: application/vnd.docker.distribution.manifest.list.v2+json               |                                                                                              
            size: 8c1d                                                                         |                                                                                              
          createdat: 010000000ed97ac3ef0aaf96deffff                                            |                                                                                              
          updatedat: 010000000ed97ac3ef0aaf96deffff                                            |                                                                                              
      - leases                                                                                 |                                                                                              
      - snapshots                                                                              |                                                                                              
        - overlayfs                                                                            |                                                                                              
          - sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4            |                                                                                              
            - children                                                                         |                                                                                              
              sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a14:         |                                                                                              
            - labels                                                                           |                                                                                              
              containerd.io/snapshot.ref: sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d12|                                                                                              
            createdat: 010000000ed97ac3ef0804dc8bffff                                          |                                                                                              
            name: default/8/sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5|                                                                                              
            parent: sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c    |                                                                                              
            updatedat: 010000000ed97ac3ef0804dc8bffff                                          |                                                                                              
          - sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f            |                                                                                              
            - children                                                                         |                                                                                              
              sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5:         |                                                                                              
            - labels                                                                           |                                                                                              
              containerd.io/snapshot.ref: sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc02907|                                                                                              
            createdat: 010000000ed97ac3a80a33892fffff                                          |                                                                                              
            name: default/2/sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c|                                                                                              
            updatedat: 010000000ed97ac3a80a33892fffff                                          |                                                                                              
          - sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5            |                                                                                              
            - children                                                                         |                                                                                              
              sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c:         |                                                                                              
            - labels                                                                           |                                                                                              
              containerd.io/snapshot.ref: sha256:780238f18c540007376dd5e904f583896a69fe620876ca|                  
```

**目录下/var/lib/containerd/io.containerd.content.v1.content/blobs/sha256**

```
[root@centos7 sha256]# sha256sum 0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31
校验结果 0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31  (文件名)0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31
```

**index 文件内容**

```
[root@centos7 sha256]# cat 0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31|python -m json.tool
{
    "manifests": [
        {
            "digest": "sha256:ee89b00528ff4f02f2405e4ee221743ebc3f8e8dd0bfd5c4c20a2fa2aaa7ede3",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "amd64",
                "os": "linux"
            },
            "size": 1570
        },
        {
            "digest": "sha256:8c299f2ab06e22c4837b1ba52d8dc7e9c24a5938c0425d470a5e4a056ff3036d",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "arm",
                "os": "linux",
                "variant": "v5"
            },
            "size": 1570
        },
        {
            "digest": "sha256:35205a452d69641a68fec5ff671b47d0c9d000877c8a9f4524f559f18fba1c9a",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "arm",
                "os": "linux",
                "variant": "v7"
            },
            "size": 1570
        },
        {
            "digest": "sha256:50ab7908620ee92646ddcd9fe321f969cdff672819fd9c8427448f67f59ef32e",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "arm64",
                "os": "linux",
                "variant": "v8"
            },
            "size": 1570
        },
        {
            "digest": "sha256:7826426c9d8d310c62fc68bcd5e8dde70cb39d4fbbd30eda3b1bd03e35fbde29",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "386",
                "os": "linux"
            },
            "size": 1570
        },
        {
            "digest": "sha256:39a9d78337db184f89d5d81a5579e3a7323e892d1d12c3b2a4a9946c258f3fe0",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "mips64le",
                "os": "linux"
            },
            "size": 1570
        },
        {
            "digest": "sha256:b4bd2a5faac23fed74bb59bb0b786bd0b4403d554fde611152cd7ddbe7023081",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "ppc64le",
                "os": "linux"
            },
            "size": 1570
        },
        {
            "digest": "sha256:83b62637ee944df8444dd064065c3ee0c39965467bfb531dcedec8339432dd40",
            "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "platform": {
                "architecture": "s390x",
                "os": "linux"
            },
            "size": 1570
        }
    ],
    "mediaType": "application/vnd.docker.distribution.manifest.list.v2+json",
    "schemaVersion": 2
}
```

**manifest文件内容**

```
[root@centos7 sha256]# cat ee89b00528ff4f02f2405e4ee221743ebc3f8e8dd0bfd5c4c20a2fa2aaa7ede3|python -m json.tool
{
    "config": {
        "digest": "sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85",
        "mediaType": "application/vnd.docker.container.image.v1+json",
        "size": 7656
    },
    "layers": [
        {
            "digest": "sha256:a2abf6c4d29d43a4bf9fbb769f524d0fb36a2edab49819c1bf3e76f409f953ea",
            "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "size": 31357624
        },
        {
            "digest": "sha256:a9edb18cadd1336142d6567ebee31be2a03c0905eeefe26cb150de7b0fbc520b",
            "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "size": 25350007
        },
        {
            "digest": "sha256:589b7251471a3d5fe4daccdddfefa02bdc32ffcba0a6d6a2768bf2c401faf115",
            "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "size": 602
        },
        {
            "digest": "sha256:186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d",
            "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "size": 894
        },
        {
            "digest": "sha256:b4df32aa5a72e2a4316aad3414508ccd907d87b4ad177abd7cbd62fa4dab2a2f",
            "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "size": 666
        },
        {
            "digest": "sha256:a0bcbecc962ed2552e817f45127ffb3d14be31642ef3548997f58ae054deb5b2",
            "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "size": 1395
        }
    ],
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "schemaVersion": 2
}
```

**config文件内容**

```
[root@centos7 sha256]# cat 605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85 |python -m json.tool
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
            "sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f",
            "sha256:e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8",
            "sha256:b8d6e692a25e11b0d32c5c3dd544b71b1085ddc1fddad08e68cbd7fda7f70221",
            "sha256:f1db227348d0a5e0b99b15a096d930d1a69db7474a1847acbc31f05e4ef8df8c",
            "sha256:32ce5f6a5106cc637d09a98289782edf47c32cb082dc475dd47cbf19a4f866da",
            "sha256:d874fd2bc83bb3322b566df739681fbd2248c58d3369cb25908d68e7ed6040a6"
        ],
        "type": "layers"
    }
}
```

**2.[root@LIN-FFF47298CDA containerd]# ctr snapshots ls 其中一个没有parent ，没有parent的是基础层的chainid,一共6层**

```
[root@LIN-FFF47298CDA containerd]# ctr snapshot tree
 sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f
  \_ sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5
    \_ sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c
      \_ sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4
        \_ sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a14
          \_ sha256:b625d8e29573fa369e799ca7c5df8b7a902126d2b7cbeb390af59e4b9e1210c5
```

**/var/lib/containerd/io.containerd.content.v1.content/blobs/sha256/目录下**

**因为pull了1个index, 1个config, 1个manifest和6个layer(解压后就是6个snapshots)**

```
[root@LIN-FFF47298CDA sha256]# file *
0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31: ASCII text, with very long lines, with no line terminators
186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d: gzip compressed data, original size 4096
589b7251471a3d5fe4daccdddfefa02bdc32ffcba0a6d6a2768bf2c401faf115: gzip compressed data, original size 3072
605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85: ASCII text, with very long lines, with no line terminators
a0bcbecc962ed2552e817f45127ffb3d14be31642ef3548997f58ae054deb5b2: gzip compressed data, original size 7168
a2abf6c4d29d43a4bf9fbb769f524d0fb36a2edab49819c1bf3e76f409f953ea: gzip compressed data, original size 83858432
a9edb18cadd1336142d6567ebee31be2a03c0905eeefe26cb150de7b0fbc520b: gzip compressed data, original size 61997056
b4df32aa5a72e2a4316aad3414508ccd907d87b4ad177abd7cbd62fa4dab2a2f: gzip compressed data, original size 3584
ee89b00528ff4f02f2405e4ee221743ebc3f8e8dd0bfd5c4c20a2fa2aaa7ede3: ASCII text
```

3.**meta 数据 /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/metadata.db （id最高是6）**

```
===============================================================================================|==============================================================================================
  - v1                                                                                         | Path: v1 → snapshots → default/2/sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9
    - parents                                                                                  | Key: inodes                                                                                  
      010002: default/4/sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5| Value: b84b                                                                                  
      020003: default/6/sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c|                                                                                              
      030004: default/8/sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4|                                                                                              
      040005: default/10/sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a1|                                                                                              
      050006: default/12/sha256:b625d8e29573fa369e799ca7c5df8b7a902126d2b7cbeb390af59e4b9e1210c|                                                                                              
    - snapshots                                                                                |                                                                                              
      - default/10/sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a14     |                                                                                              
        - labels                                                                               |                                                                                              
          containerd.io/snapshot.ref: sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed0|                                                                                              
        createdat: 010000000ed97ac3ef090d39a6ffff                                              |                                                                                              
        id: 05                                                                                 |                                                                                              
        inodes: 06                                                                             |                                                                                              
        kind: 03                                                                               |                                                                                              
        parent: default/8/sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1|                                                                                              
        size: 8040                                                                             |                                                                                              
        updatedat: 010000000ed97ac3ef090d39a6ffff                                              |                                                                                              
      - default/12/sha256:b625d8e29573fa369e799ca7c5df8b7a902126d2b7cbeb390af59e4b9e1210c5     |                                                                                              
        + labels                                                                               |                                                                                              
        createdat: 010000000ed97ac3ef0a016a20ffff                                              |                                                                                              
        id: 06                                                                                 |                                                                                              
        inodes: 06                                                                             |                                                                                              
        kind: 03                                                                               |                                                                                              
        parent: default/10/sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31|                                                                                              
        size: 808001                                                                           |                                                                                              
        updatedat: 010000000ed97ac3ef0a016a20ffff                                              |                                                                                              
      - default/2/sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f      |                                                                                              
        - labels                                                                               |                                                                                              
          containerd.io/snapshot.ref: sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41|                                                                                              
        createdat: 010000000ed97ac3a80db6dc36ffff                                              |                                                                                              
        id: 01                                                                                 |                                                                                              
        inodes: b84b                                                                           |                                                                                              
        kind: 03                                                                               |                                                                                              
        size: 8080ee55                                                                         |                                                                                              
        updatedat: 010000000ed97ac3a80db6dc36ffff                                              |                                                                                              
      - default/4/sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5      |                                                                                              
        - labels                                                                               |                                                                                              
          containerd.io/snapshot.ref: sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06|                                                                                              
        createdat: 010000000ed97ac3ef05ab5c22ffff                                              |                                                                                              
        id: 02                                                                                 |                                                                                              
        inodes: f814                                                                           |                                                                                              
        kind: 03                                                                               |                                                                                              
        parent: default/2/sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c85|                                                                                              
        size: 80c0ab3c                                                                         |                                                                                              
        updatedat: 010000000ed97ac3ef05ab5c22ffff   
      - default/6/sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c      |                                                                                              
        - labels                                                                               |                                                                                              
          containerd.io/snapshot.ref: sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b|                                                                                              
        createdat: 010000000ed97ac3ef0700a48fffff                                              |                                                                                              
        id: 03                                                                                 |                                                                                              
        inodes: 04                                                                             |                                                                                              
        kind: 03                                                                               |                                                                                              
        parent: default/4/sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4f|                                                                                              
        size: 8040                                                                             |                                                                                              
        updatedat: 010000000ed97ac3ef0700a48fffff                                              |                                                                                              
      - default/8/sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4      |                                                                                              
        - labels                                                                               |                                                                                              
          containerd.io/snapshot.ref: sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d122744|                                                                                              
        createdat: 010000000ed97ac3ef0807bd5affff                                              |                                                                                              
        id: 04                                                                                 |                                                                                              
        inodes: 06                                                                             |                                                                                              
        kind: 03                                                                               |                                                                                              
        parent: default/6/sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe|                                                                                              
        size: 8040                                                                             |                                                                                              
        updatedat: 010000000ed97ac3ef0807bd5affff                                               |                            
```

**4.在/var/lib/containerd/io.containerd.content.v1.content/blobs/sha256/ 目录下**

**grep -rnwI "diff_ids"**

**找到 io.containerd.content.v1.content/blobs/sha256/605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85**

**cat io.containerd.content.v1.content/blobs/sha256/605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85|python -m json.tool**

```
        ....
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
            "sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f",
            "sha256:e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8",
            "sha256:b8d6e692a25e11b0d32c5c3dd544b71b1085ddc1fddad08e68cbd7fda7f70221",
            "sha256:f1db227348d0a5e0b99b15a096d930d1a69db7474a1847acbc31f05e4ef8df8c",
            "sha256:32ce5f6a5106cc637d09a98289782edf47c32cb082dc475dd47cbf19a4f866da",
            "sha256:d874fd2bc83bb3322b566df739681fbd2248c58d3369cb25908d68e7ed6040a6"
        ],
        "type": "layers"
    }
}
```

**5.计算公式**

**最底层：chainID=diffID**

**其他层公式：chainID(n)=sha256sum(chainID(n-1)) diffID(n))**

chainID(0)=sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f =diffID (0)

chain(1)= sha256sum(chainid(0)  diffID(1))

chainID(0)=sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f

已知diffID(1)=sha256:e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f**02421ad8

```
[root@LIN-FFF47298CDA containerd]# echo -n 'sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f sha256:e379e8aedd4d72bb4c529a4ca07a4e4d230b5a1d3f7a61bc80179e8f02421ad8' | sha256sum
780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5  -
```

同理计算

chanID(1)=780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5

查找meta 数据  /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/metadata.db

id是02  对应 /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/snapshots/2

chanID(2)=b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c

id是03  对应 /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/snapshots/3

chanID(3)=02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4

**6.镜像层还原校验在/var/lib/containerd/io.containerd.content.v1.content/blobs/sha256目录**

复制到其他目录186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d 改为

186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d.gz

（1）gzip -d 186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d.gz 生成186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d (tar文件类型和之前的不同之前是gz类型虽然名字一样)

（2）mkdir ./x

然后 执行命令生成 tar-data.json.gz

**tar-split disasm --output tar-data.json.gz ./186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d | tar -C ./x -x**

（3）测试：tar-split disasm --output tar-data.json.gz ./archive.tar | tar -C ./x -x 生成new.tar

[root@centos7 nmx]# sha256sum new.tar
f1db227348d0a5e0b99b15a096d930d1a69db7474a1847acbc31f05e4ef8df8c new.tar

**根据 chain id（3）02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4**

**metadata.db找到 id是04因此**

**path=/var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/snapshots/4/fs/(要有fs)**

**（4）生成 new1.tar 并校验**

```
[root@centos7 nmx]# tar-split asm --output new1.tar --input ./tar-data.json.gz --path /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/snapshots/4/fs/
INFO[0000] created new1.tar from /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/snapshots/4/fs/ and ./tar-data.json.gz (wrote 4096 bytes)
```

```
[root@centos7 nmx]# sha256sum new1.tar
f1db227348d0a5e0b99b15a096d930d1a69db7474a1847acbc31f05e4ef8df8c  new1.tar校验
```

**校验结果是f1db227348d0a5e0b99b15a096d930d1a69db7474a1847acbc31f05e4ef8df8c是diffID(3)**
