# Linux没有ROOT权限如何安装软件


## Conda

## 环境变量

```bash
### Initiative
PS1='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w \$\[\033[00m\] '
export PATH=~/local/bin:~/bin:$PATH
#export LD_LIBRARY_PATH=~/local/lib:$LD_LIBRARY_PATH
export CORE=$(grep "processor" /proc/cpuinfo|sort -u|wc -l)
#解决Perl、Python、MongoDB 等 Shell 中 Backspace 键出现 ^H 问题
stty erase ^H
```

## Python
```shell
wget https://www.python.org/ftp/python/3.5.1/Python-3.5.1.tgz
./configure --prefix=$HOME/local --enable-shared -with-threads --enable-ipv6 \
&& make -j $core && make install -j $CORE
```

## Perl
### Perl5

```bash
wget http://www.cpan.org/src/5.0/perl-5.26.0.tar.gz
tar -zxf perl-5.26.0.tar.gz && cd perl-5.26.0
sh Configure -de -Dprefix=$HOME/local && make -j $CORE && make install
```
### cpan安装perl模块

```shell
# MCPAN 方案
perl -MCPAN -e shell
cpan>h #help
cpan>i /scws/ #search modules
cpan>install <moduleName>
#### cpan精简版 ####
#相关参数可以查看帮助文档 perldoc -F $HOME/local/bin/cpan
cpan -i Module::Name
#perl命令行版
perl -MCPAN -e 'install CJFIELDS/BioPerl-1.6.924.tar.gz'
```

### cpanm安装perl模块
cpanm可能是是安装Perl模块的最简洁的方法，很好滴解决模块之间的依赖关系
而且输出不像cpan那么繁琐，更为友好地告诉你它在干什么
`cpanm`就是一个可执行文件，将它下载到你的bin目录，然后添加执行权限就可以了。

```shell
wget http://xrl.us/cpanm -O /usr/bin/cpanm && chmod +x /usr/bin/cpanm
```
使用cpanm安装模块
```shell
# cpanm -h
  -v,--verbose              Turns on chatty output
  -q,--quiet                Turns off the most output
  --interactive             开启交互配置(required for Task:: modules)
  -f,--force                强制安装
  -n,--notest               Do not run unit tests
  --test-only               只测试不安装
  -S,--sudo                 sudo to run install commands
  --installdeps             只安装依赖模块
  --showdeps                只显示依赖信息
  --reinstall               重新安装
  --mirror                  指定镜像url (e.g. http://cpan.cpantesters.org/)
  --mirror-only             只从镜像下载
  --prompt                  Prompt when configure/build/test fails
  -l,--local-lib            Specify the install base to install modules
  -L,--local-lib-contained  Specify the install base to install all non-core modules
  --self-contained          Install all non-core modules, even if they\'re already installed.
  --auto-cleanup            Number of days that cpanm\'s work directories expire in. Defaults to 7
```
例子
```
cpanm Test::More                                    # install Test::More
cpanm MIYAGAWA/Plack-0.99_05.tar.gz                 # full distribution path
cpanm http://example.org/LDS/CGI.pm-3.20.tar.gz     # install from URL
cpanm ~/dists/MyCompany-Enterprise-1.00.tar.gz      # install from a local file
cpanm --interactive Task::Kensho                    # Configure interactively
cpanm .                                             # install from local directory
cpanm --installdeps .                               # install all the deps for the current directory
cpanm -L extlib Plack                               # install Plack and all non-core deps into extlib
cpanm --mirror http://cpan.cpantesters.org/ DBI     # use the fast-syncing mirror
```



