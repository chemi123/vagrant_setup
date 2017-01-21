# dockerコマンド等まとめ

1. イメージ  
2. コンテナ  
3. イメージの作成

## 1. イメージ
### イメージの検索
例えばcentosイメージを検索する場合
```
$ sudo docker search centos
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                                 The official build of CentOS.                   3021      [OK]
jdeathe/centos-ssh                     CentOS-6 6.8 x86_64 / CentOS-7 7.3.1611 x8...   56                   [OK]
nimmis/java-centos                     This is docker images of CentOS 7 with dif...   22                   [OK]
consol/centos-xfce-vnc                 Centos container with "headless" VNC sessi...   18                   [OK]
million12/centos-supervisor            Base CentOS-7 with supervisord launcher, h...   12                   [OK]
nickistre/centos-lamp                  LAMP on centos setup                            9                    [OK]
torusware/speedus-centos               Always updated official CentOS docker imag...   8                    [OK]
...以下出力結果
```

### イメージの取得
searchでcentosという名前のイメージが見つかったので取得してみる
```
$ sudo docker pull centos

// イメージ一覧を取得
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              48b5124b2768        5 days ago          1.84 kB
centos              latest              67591570dd29        4 weeks ago         191.8 MB  // centosがあることを確認
```

### イメージの詳細を表示
```
// タグを指定する場合
$ sudo docker inspect centos:latest //${イメージ名}:${タグ名} と指定する。タグ名を指定しない場合はデフォルトでlatestとなる

// image idを指定する場合
$ sudo docker inspect ${image_id}
```

### イメージの削除
```
$ sudo docker rmi ${image_id}
```

## 2. コンテナ
### コンテナの起動
このコマンドでの`centos`はイメージ名、以下今回のサンプルではイメージを全てcentosで実行する
```
// centosイメージからコンテナを作成し、そのコンテナ上で`echo "hello world"`を実行　
$ sudo docker run centos echo "hello world"
```

#### バックグランド実行
`free -s 3`部分はコマンド部分
```
$ sudo docker run -d centos free -s 3
```

#### コンテナのプロセスを確認
```
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
b6a0a8ce1a30        centos              "free -s 3"         3 seconds ago       Up 1 seconds                            drunk_meitner

// -aオプションを付けることで過去に実行完了したコンテナのプロセスを確認できる
$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                        PORTS               NAMES
b6a0a8ce1a30        centos              "free -s 3"            2 minutes ago       Up 2 minutes                                      drunk_meitner
25e793cb299b        centos              "echo hoge"            11 minutes ago      Exited (0) 11 minutes ago                         clever_rosalind
```

#### バックグランドで行われている処理をフォアグラウンドにもってくる
```
// --sig-proxy=falseをつけることでctrl+Cでcontainerをkillせずにバックグランド実行に戻せる
$ sudo docker attach --sig-proxy=false ${contaier_id}
```

#### コンテナのログを確認
```
$ sudo docker log ${container_id}
```

### コンテナの停止
```
以下二通り
$ sudo docker kill ${container_id}
$ sudo docker stop ${container_id}

// 再び起動させるには
$ sudo docker start ${container_id}
```

### コンテナの削除
```
$ sudo docker rm ${container_id}
```

### コンテナにbashでログインする
```
// -i: インタラクティブモード
// -t: ターミナルを指定(イメージ名の後)
$ sudo docker run -i -t centos /bin/bash 
```

## 3. イメージの作成　
### イメージの元となるコンテナの作成
例としてコンテナにbashでログインした後に以下パッケージのインストール及び設定を行う  
- vim, zshのインストール
- useraddで`chemi`を追加
- root, chemiのログインシェルをzshに変更

終わったら一旦exitでターミナルから抜けてコンテナidを確認する
```
$ sudo docker ps -a
[vagrant@localhost]$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                        PORTS               NAMES
556a332e7449        centos              "/bin/bash"            9 minutes ago       Exited (0) 4 seconds ago                          mad_minsky
5a88dbabc687        centos              "/bin/bash"            14 minutes ago      Exited (0) 13 minutes ago                         clever_elion
...
...
...
```

コンテナidを使ってイメージを作成する。コンテナidの後に来る名前は慣習として${ユーザ名}/${わかりやすい名前}を使う模様
```
$ sudo docker commit ${container_id} chemi/init // 少し時間かかる
```

作成したイメージを使ってコンテナを立ち上げる
```
$ sudo docker images
// イメージが出来ていることを確認

$ sudo docker run chemi/init echo "hoge"
hoge
```
