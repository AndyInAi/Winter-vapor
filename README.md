# Winter-vapor

```sh

echo "
  deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
  deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
  deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
  deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
" > /etc/apt/sources.list

export DEBIAN_FRONTEND=noninteractive

apt update -y

apt install -y git xz-utils

if [ ! -d ~/Winter-vapor ]; then cd ~/ ; git clone https://github.com/AndyInAi/Winter-vapor.git; fi


# 运行

cd  ~/Winter-vapor

xz -d -p App.xz

chmod +x ./App

./App serve -b 0.0.0.0:8080

# starting HTTP server at http://0.0.0.0:8080/


# 或者安装编译环境，编译源代码后运行

if [ ! -f /usr/bin/swift ]; then

	_sum=""
	if [ -f ~/swift-5.9.1-RELEASE-ubuntu22.04.tar.gz ]; then
		_sum="`sha256sum ~/swift-5.9.1-RELEASE-ubuntu22.04.tar.gz | \
		grep ea2fe1190a9cb8abed8c5e7b94223a06a23c7dc8cd498850a1c79b8a87e7c251`"
	fi

	if [ "$_sum" == "" ]; then
	  wget  -O ~/swift-5.9.1-RELEASE-ubuntu22.04.tar.gz \
	  https://download.swift.org/swift-5.9.1-release/ubuntu2204/swift-5.9.1-RELEASE/swift-5.9.1-RELEASE-ubuntu22.04.tar.gz
	fi

	if [ ! -d swift-5.9.1-RELEASE-ubuntu22.04 ]; then
		tar xvzf ~/swift-5.9.1-RELEASE-ubuntu22.04.tar.gz -C ~/
	fi

	if [ -d swift-5.9.1-RELEASE-ubuntu22.04 ]; then
		cp -r -f /root/swift-5.9.1-RELEASE-ubuntu22.04/* /
	fi

fi

if [ ! -d ~/toolbox ]; then
	cd ~/
	git clone https://github.com/vapor/toolbox.git
fi

if [ -d ~/toolbox ]; then
	cd ~/toolbox
	git checkout 18.7.4
	make install
fi

# 编译源代码

cd  ~/Winter-vapor

swift build -c release --static-swift-stdlib

# 运行

./.build/release/App serve -b 0.0.0.0:8080

# starting HTTP server at http://0.0.0.0:8080/

```
