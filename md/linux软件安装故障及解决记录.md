### Redis
> tar.gz包解压后make时报错：jemalloc

    # 解决
    make MALLOC=libc
---
### ruby
> 编译安装新版本

    # 要求,系统已安装openssl,openssl-devel,zlib
    ./configure
    make && make install
    
> 安装redis-trib(要求ruby版本在2.2以上)

    gem install redis
