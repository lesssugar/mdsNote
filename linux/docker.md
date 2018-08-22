# 地址

http://docker-cn.com/

centos 7版本的使用可以参照文档

# 概念

- 镜像 镜像就相当于java中的类，为只读模板
- 容器 独立运行的一个或一组应用，就是通过镜像所生成的实例，可以进行启动停止删除的操作。每个都是相互隔离的安全环境
- 仓库是集中存放镜像的地方

# 安装

```bash
yum install -y epel-release		//docker的依赖库
```

# 配置文件

```bash
vi /etc/sysconfig/docker	//配置文件地址
```

# 使用

```bash
service docker start
docker run hello-world	//会先运行本地的镜像，如果没有会自动从仓库中拉取
docker info //详细信息
docker --help
```

## 阿里云镜像

```bash
https://dev.aliyun.com/search.html
6.8
vim /etc/sysconfig/docker
other_args="--registry-mirror=https://abs6m4jv.mirror.aliyuncs.com"

7.0++
vim /etc/docker/daemon.json
{
  "registry-mirrors": ["https://abs6m4jv.mirror.aliyuncs.com"]
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 镜像的命令

```bash
docker images	//显示所有的镜像信息
				-a	//镜像是多层的。显示所有层
				-q	//只显示当前镜像的id	//-aq可以查询出该镜像的全部id，可以批量进行删除
				--digests	//镜像的摘要信息
				--no-trunc	//显示id的完整信息
docker search	//按名称查询需要的镜像名字，类似npm search
				-s 30	//把点赞数超过30的镜像查询出来
docker pull tomcat
docker rmi hello-world
docker rmi ${docker images -qa}	//删除某个镜像的全部镜像
docker commit //提交容器副本成为一个新的镜像
docker commit -a="wyq" -m="del docs tomcat" bac73cf399c4  wyq/tomcat:0.1	//将正在运行的镜像打包成一个新的镜像	
```

# 容器的操作

```bash
docker run [] IMAGE 镜像名 []
						-i	//以交互的形式启动容器，通常it一起使用
						-t	//重新分配一个伪输入终端
						--name	//给容器分配的名称
						-d		//不进行交互，以后台守护进程的方式运行容器，dockee ps查询不到，没有前台进行docker就自动对齐进行了关闭
//可以通过shell编程的方式。没两秒进行一次操作，后台就可以查询到持续运行的容器
docker run -d centos 	/bin.sh -c "while true;do echo xxx;sleep 2;done"
docker ps	 []	//查看那些容器正在运行
			-a	//查看现在运行的和历史运行过的
			-l	//查看上次运行的容器
			-n 3	//最近三次使用过的容器	
			-q	//只显示容器编号，批量删除容器
exit	//退出，会关闭容器并退出
ctrl + p + q	//
docker start 容器id	//重新回到
docker restart id	//讲一个运行中的容器进行重启
docker stop id		//慢慢重启
docker kill id		//马上关闭
docker rm 	id		//删除一个已经停止的容器
docker rm -f $(docker ps -aq)	或者	docker ps -aq|args docker rm
docker top	id	//查看某个docker中的进程
docker inspect id	//以json的形式描述整个docker
docker attach  id	//重新进入某个docker容器
docker exec -t id ls /l /tmp	//在宿主机也就是容器外部对某个docker容器进行操作
docker cp id 文件路径	主机路径	//复制docker的文件到主机中
```

## 例子

```bash
docker run -it centos //进入了一个新的centos系统
docker logs [] 容器id	//显示一个容器的日志
			-t //时间戳
			-f //不停的追加
			--tail	//最后几条
docker run -it -p 8888:8080 tomcat //主机外部端口，对应内部8080端口
			  -P tomcat		//端口号随机分配，可以通过docker ps查看
docker exec -it bac73cf399c4 /bin/bash	//启动tomcat后进入容器的路径
```

# 镜像

UnionFS	联合文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂在到同一个虚拟文件系统下，特性，一次可以加载多个文件系统，但在外层只能看到一个文件系统，比如一个tomcat，最内层kernel,centos,java,tomcat

## bootfs

主要包含bootloader和kernel,bootloader主要作用是加载kernel,Linux刚加载的时候会加载bootfs文件系统，在Docker的最底层是bootfs，当boot加载整个内核到内存中后，将内存的所有权交给内核，此时卸载bootfs

## rootfs

在bootfs之上就是rootfs，包含典型Linux系统中的/dev，/proc,/bin，/etc等标准文件和目录，rootfs就是各种	不同的操作系统发行版，如Ubantu,centos,精简的OS,rootfs可以很小，只需要包含基本的命令，工具程序库即可，因为底层直接用Host的kernel,自己只需要提供rootfs即可，由此可见对于不同的系统，bootfs基本一致，rootfs千差万别，因此不同的系统可以共用一个bootfs

# 容器数据卷

一些容器运行的时候产生的数据我们希望对齐进行持久化，就使用数据卷来进行保存

- 数据卷可以再容器之间共享或者重用数据
- 卷中的更改可以直接生效
- 数据卷中的更新不会直接包含在镜像中
- 数据卷的生命周期会一直持续到没有容器使用它为止

## 添加方式

命令行添加

```bash
docker run -it -v /宿主机绝对路径：/容器内目录 镜像名	//v自动新建文件夹
两个路径产生的是两个文件夹，主机容器中进行的修改都会在另一方产生影响，即使docker关闭，主机修改还是会同步到容器
docker run -it -v /宿主机绝对路径：/容器内目录 镜像名:ro	//v自动新建文件夹
ro	rean only	//只读，该种情况下数据只允许主机单向的进行修改，容器不允许修改
```

dockerFile添加

```bash
创建DockerFile 文件
# volume test
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished ----- success1"
CMD /bin/bash
相当于
docker -run -it -v /host1:/dataVolumeContainer1 -v/host2:/dataVolumeContainer2 centos /bin/bash
docker build -f /mydocker/DockerFile -t wyq/centos .	//通过DockerFile构建镜像，docker会自己生成默认主机数据卷
如果碰到 cannot open directory.:Permission denied错误，在挂在目录后面加 --privileged=true即可	
```

## 数据卷容器

一个数据卷通过挂在在另一个数据卷上来达到数据的共享



# DockerFile

```xml
FROM openjdk:7-jre	//镜像来自于哪

ENV CATALINA_HOME /usr/local/tomcat		//tomcat路径
ENV PATH $CATALINA_HOME/bin:$PATH		
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

# let "Tomcat Native" live somewhere isolated
ENV TOMCAT_NATIVE_LIBDIR $CATALINA_HOME/native-jni-lib
ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}$TOMCAT_NATIVE_LIBDIR

# runtime dependencies for Tomcat Native Libraries
# Tomcat Native 1.2+ requires a newer version of OpenSSL than debian:jessie has available
# > checking OpenSSL library version >= 1.0.2...
# > configure: error: Your version of OpenSSL is not compatible with this version of tcnative
# see http://tomcat.10.x6.nabble.com/VOTE-Release-Apache-Tomcat-8-0-32-tp5046007p5046024.html (and following discussion)
# and https://github.com/docker-library/tomcat/pull/31
ENV OPENSSL_VERSION 1.1.0f-3+deb9u2
RUN set -ex; \
	currentVersion="$(dpkg-query --show --showformat '${Version}\n' openssl)"; \
	if dpkg --compare-versions "$currentVersion" '<<' "$OPENSSL_VERSION"; then \
		if ! grep -q stretch /etc/apt/sources.list; then \
# only add stretch if we're not already building from within stretch
			{ \
				echo 'deb http://deb.debian.org/debian stretch main'; \
				echo 'deb http://security.debian.org stretch/updates main'; \
				echo 'deb http://deb.debian.org/debian stretch-updates main'; \
			} > /etc/apt/sources.list.d/stretch.list; \
			{ \
# add a negative "Pin-Priority" so that we never ever get packages from stretch unless we explicitly request them
				echo 'Package: *'; \
				echo 'Pin: release n=stretch*'; \
				echo 'Pin-Priority: -10'; \
				echo; \
# ... except OpenSSL, which is the reason we're here
				echo 'Package: openssl libssl*'; \
				echo "Pin: version $OPENSSL_VERSION"; \
				echo 'Pin-Priority: 990'; \
			} > /etc/apt/preferences.d/stretch-openssl; \
		fi; \
		apt-get update; \
		apt-get install -y --no-install-recommends openssl="$OPENSSL_VERSION"; \
		rm -rf /var/lib/apt/lists/*; \
	fi

RUN apt-get update && apt-get install -y --no-install-recommends \
		libapr1 \
	&& rm -rf /var/lib/apt/lists/*

# see https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/KEYS
# see also "update.sh" (https://github.com/docker-library/tomcat/blob/master/update.sh)
ENV GPG_KEYS 05AB33110949707C93A279E3D3EFE6B686867BA6 07E48665A34DCAFAE522E5E6266191C37C037D42 47309207D818FFD8DCD3F83F1931D684307A10A5 541FBE7D8F78B25E055DDEE13C370389288584E7 61B832AC2F1C5A90F0F9B00A1C506407564C17A3 713DA88BE50911535FE716F5208B0AB1D63011C7 79F7026C690BAA50B92CD8B66A3AD3F4F22C4FED 9BA44C2621385CB966EBA586F72C284D731FABEE A27677289986DB50844682F8ACB77FC2E86E29AC A9C5DF4D22E99998D9875A5110C01C5A2F6059E7 DCFD35E0BF8CA7344752DE8B6FB21E8933C60243 F3A04C595DB5B6A5F1ECA43E3B7BBB100D811BBE F7DA48BB64BCB84ECBA7EE6935CD23C10D498E23

ENV TOMCAT_MAJOR 8
ENV TOMCAT_VERSION 8.0.53
ENV TOMCAT_SHA512 cd8a4e48a629a2f2bb4ce6b101ebcce41da52b506064396ec1b2915c0b0d8d82123091242f2929a649bcd8b65ecf6cd1ab9c7d90ac0e261821097ab6fbe22df9

ENV TOMCAT_TGZ_URLS \
# https://issues.apache.org/jira/browse/INFRA-8753?focusedCommentId=14735394#comment-14735394
	https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
# if the version is outdated, we might have to pull from the dist/archive :/
	https://www-us.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
	https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
	https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz

ENV TOMCAT_ASC_URLS \
	https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
# not all the mirrors actually carry the .asc files :'(
	https://www-us.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
	https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
	https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc

RUN set -eux; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get update; \
	\
	apt-get install -y --no-install-recommends gnupg dirmngr; \
	\
	export GNUPGHOME="$(mktemp -d)"; \
	for key in $GPG_KEYS; do \
		gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done; \
	\
	apt-get install -y --no-install-recommends wget ca-certificates; \
	\
	success=; \
	for url in $TOMCAT_TGZ_URLS; do \
		if wget -O tomcat.tar.gz "$url"; then \
			success=1; \
			break; \
		fi; \
	done; \
	[ -n "$success" ]; \
	\
	echo "$TOMCAT_SHA512 *tomcat.tar.gz" | sha512sum -c -; \
	\
	success=; \
	for url in $TOMCAT_ASC_URLS; do \
		if wget -O tomcat.tar.gz.asc "$url"; then \
			success=1; \
			break; \
		fi; \
	done; \
	[ -n "$success" ]; \
	\
	gpg --batch --verify tomcat.tar.gz.asc tomcat.tar.gz; \
	tar -xvf tomcat.tar.gz --strip-components=1; \
	rm bin/*.bat; \
	rm tomcat.tar.gz*; \
	command -v gpgconf && gpgconf --kill all || :; \
	rm -rf "$GNUPGHOME"; \
	\
	nativeBuildDir="$(mktemp -d)"; \
	tar -xvf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1; \
	apt-get install -y --no-install-recommends \
		dpkg-dev \
		gcc \
		libapr1-dev \
		libssl-dev \
		make \
		"openjdk-${JAVA_VERSION%%[.~bu-]*}-jdk=$JAVA_DEBIAN_VERSION" \
	; \
	( \
		export CATALINA_HOME="$PWD"; \
		cd "$nativeBuildDir/native"; \
		gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
		./configure \
			--build="$gnuArch" \
			--libdir="$TOMCAT_NATIVE_LIBDIR" \
			--prefix="$CATALINA_HOME" \
			--with-apr="$(which apr-1-config)" \
			--with-java-home="$(docker-java-home)" \
			--with-ssl=yes; \
		make -j "$(nproc)"; \
		make install; \
	); \
	rm -rf "$nativeBuildDir"; \
	rm bin/tomcat-native.tar.gz; \
	\
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*; \
	\
# sh removes env vars it doesn't support (ones with periods)
# https://github.com/docker-library/tomcat/issues/77
	find ./bin/ -name '*.sh' -exec sed -ri 's|^#!/bin/sh$|#!/usr/bin/env bash|' '{}' +

# verify Tomcat Native is working properly
RUN set -e \
	&& nativeLines="$(catalina.sh configtest 2>&1)" \
	&& nativeLines="$(echo "$nativeLines" | grep 'Apache Tomcat Native')" \
	&& nativeLines="$(echo "$nativeLines" | sort -u)" \
	&& if ! echo "$nativeLines" | grep 'INFO: Loaded APR based Apache Tomcat Native library' >&2; then \
		echo >&2 "$nativeLines"; \
		exit 1; \
	fi

EXPOSE 8080		//对外暴露的端口
CMD ["catalina.sh", "run"]	
```

