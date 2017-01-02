# dockerのインストール手順及びメモ

## インストール(yum)
[ここ](https://docs.docker.com/engine/installation/linux/centos/)を参考にインストール。  
一応以下にコマンドも載せておく。  

### 1. 64-bit OSでかつversion 3.10以上であることを確認
```
$ uname -r
```

### 2. yumのアップデート　
```
$ sudo yum update -y
```

### 3. yum repoを追加する
```
$ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

### 4. dockerパッケージのインストール
```
$ sudo yum install docker-engine -y

// 起動
$ sudo systemctl start docker

// 自動起動on
$ sudo systemctl enable docker

// test imageを実行することで正しくインストールされているか確認
$ sudo docker run --rm hello-world
// 以下メッセージが出力されたら成功
ble to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world

78445dd45222: Pull complete
Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```
