---
layout: post
title: "Configs like Comments"
date: 2014-02-21 00:43
comments: true
categories: 
---

There are times you deliver procedures or source code to people with
certain allergies.  I am thinking of allergies making them feel bad or
uncomfortable when seeing **modern** approaches or technologies.

Referencing to lists of **big enterprises** using this method doesn't help,
but should make you realize this technology is already more
evergreen than modern... (enterprises).

However, let's not loose our happiness and sustain the mean creativity
phase for some minutes to make the best with accepted building blocks.

Deployment without **Puppet & Chef**
------------------------------------

...or building blocks you can use, also with Puppet & Co later anyway.

That's even less a pain after you've realized pure puppet is neither
good for some sequencial but complicated deployment areas.

But we start with an area, that definitely is core puppet area:
system configuration.

``` bash s1-config-sys.sh https://gist.github.com/kapfenho/9126365 Gist
#!/bin/bash
 
# Installation script for Oracle Identity and Access Management
# 
# This procedure will create the root script to execute before the
#+software installation.  You need to modify etc/configs.sh to
#+your needs.  Content of the root scripts:
#+ * user and group
#+ * base directory
#+ * system packages for fedora/redhat.
#
_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
. ${_DIR}/config/files.sh
. ${_DIR}/lib/deployers.sh
 
FEDORA_EPEL="${MISC_DIR}/epel-release-6-8.noarch.rpm"
 
echo "#!/usr/bin/env bash"
echo "#  execute script output as root user"
echo
 
# groups     name       id
create_group 'oinstall' 6001
create_group 'dba'      6002
create_group 'idm'      6003
create_group 'iam'      6004
 
create_user  'oracle' 5001  '6002'  '6001'       # database
create_user  'idm'    5002  '6003'  '6001'       # oid, ovd
create_user  'iam'    5003  '6004'  '6001'       # oam, oim
 
sudo_for     'oracle'
 
dir_for      '/appl/dbs'        'oracle:dba'
dir_for      '/appl/idm'        'idm:idm'
dir_for      '/appl/iam'        'iam:iam'
dir_for      '/appl/logs/dbs'   'oracle:dba'
dir_for      '/appl/logs/idm'   'idm:idm'
dir_for      '/appl/logs/iam'   'iam:iam'
 
set_sysctl   'kernel.msgmnb'                 '65536'
set_sysctl   'kernel.msgmnb'                 '65536'
set_sysctl   'kernel.msgmax'                 '65536'
set_sysctl   'kernel.shmmax'                 '2588483584'
set_sysctl   'kernel.shmall'                 '2097152'
set_sysctl   'kernel.shmmni'                 '4096'
set_sysctl   'kernel.sem'                    '250 32000 100 128'
 
set_sysctl   'fs.file-max'                   '6815744'
set_sysctl   'fs.aio-max-nr'                 '1048576'
 
set_sysctl   'net.ipv4.tcp_keepalive_time'   '1800'
set_sysctl   'net.ipv4.tcp_keepalive_intvl'  '30'
set_sysctl   'net.ipv4.tcp_keepalive_probes' '5'
set_sysctl   'net.ipv4.tcp_fin_timeout'      '30'
set_sysctl   'net.ipv4.ip_local_port_range'  '9000 65500'
 
set_sysctl   'net.core.rmem_default'         '262144'
set_sysctl   'net.core.rmem_max'             '4194304'
set_sysctl   'net.core.wmem_default'         '262144'
set_sysctl   'net.core.wmem_max'             '1048576'
 
activate_sysctl
 
set_limit    '*          soft    nofile      2048' 
set_limit    '*          hard    nofile      8192' 
set_limit    '@oracle    soft    nofile      65536'
set_limit    '@oracle    hard    nofile      65536'
set_limit    '@oracle    soft    nproc       2048' 
set_limit    '@oracle    hard    nproc       16384' 
set_limit    '@oracle    soft    stack       10240'
 
packs=(
    binutils.x86_64
    compat-libcap1.x86_64
    compat-libstdc++-33.i686
    compat-libstdc++-33.x86_64
    elfutils-libelf-devel.x86_64
    gcc-c++.x86_64
    gcc.x86_64
    glibc-devel.i686
    glibc-devel.x86_64
    glibc.x86_64
    glibc.i686
    ksh.x86_64
    libaio-devel.i686
    libaio-devel.x86_64
    libaio.x86_64
    libaio.i686
    libgcc.x86_64
    libstdc++-devel.i686
    libstdc++-devel.x86_64
    libstdc++.i686
    libstdc++.x86_64
    libXext.i686
    libXtst.i686
    libXi.i686
    make.x86_64
    openmotif.x86_64
    openmotif22.x86_64
    redhat-lsb-core.x86_64
    sysstat.x86_64
    unixODBC-devel
    unzip
    rlwrap )
 
add_epel_rpm ${FEDORA_EPEL}
 
add_packages packs
 
disable_service   'iptables'
disable_service   'ip6tables'
 
exit 0
```

Funtion lib: [`deployer.sh`](https://gist.github.com/kapfenho/9126296)

Just an idea - applying a domain specific language (DSL) approach with
shell scripting, while trying not to get to complex.

There's some kind of beauty - even done in bash, isn't it? 

_no comment section available..._

