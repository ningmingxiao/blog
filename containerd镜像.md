**1. nerdctl  pull nginx:1.21**

**/var/lib/containerd/io.containerd.metadata.v1.bolt目录下查看**

**boltbrowser 1meta.db （meta.db的复制）**

```
                                                                                     boltbrowser: 1meta.db                                                                                    
===============================================================================================|==============================================================================================
  - v1                                                                                         | Path: v1 → default → content → ingests                                                       
    - default                                                                                  | Buckets: 0                                                                                   
      - content                                                                                | Pairs: 0                                                                                     
        - blob                                                                                 |                                                                                              
          + sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31            |                                                                                              
          + sha256:186b1aaa4aa6c480e92fbd982ee7c08037ef85114fbed73dbb62503f24c1dd7d            |                                                                                              
          + sha256:589b7251471a3d5fe4daccdddfefa02bdc32ffcba0a6d6a2768bf2c401faf115            |                                                                                              
          + sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85            |                                                                                              
          + sha256:a0bcbecc962ed2552e817f45127ffb3d14be31642ef3548997f58ae054deb5b2            |                                                                                              
          + sha256:a2abf6c4d29d43a4bf9fbb769f524d0fb36a2edab49819c1bf3e76f409f953ea            |                                                                                              
          + sha256:a9edb18cadd1336142d6567ebee31be2a03c0905eeefe26cb150de7b0fbc520b            |                                                                                              
          + sha256:b4df32aa5a72e2a4316aad3414508ccd907d87b4ad177abd7cbd62fa4dab2a2f            |                                                                                              
          + sha256:ee89b00528ff4f02f2405e4ee221743ebc3f8e8dd0bfd5c4c20a2fa2aaa7ede3            |                                                                                              
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
