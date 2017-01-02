## vagrantの操作メモ

### 前提としてインストールされているべきもの
1. virtualbox
2. vagrant

### 操作
- boxのインストール
- vagrantの初期化/起動
- 状態の確認
- 一時停止/起動
- virtual machineの削除
- boxの作成


### boxのインストール
[ここ](http://www.vagrantbox.es/)でboxのダウンロードリンクをどれかしらコピー。  
この例ではCentOS 6.7 x64で行う。boxの名前はcentos64とする。
```
$ vagrant box add centos64 https://github.com/CommanderK5/packer-centos-template/releases/download/0.6.7/vagrant-centos-6.7.box
```

以下コマンドでインストールされているbox一覧を確認できる
```
$ vagrant box list
```

### vagrantの初期化/起動
適当に作業用フォルダを作って移動した上で以下コマンドを実行する。  
```
$ vagrant init centos64
```
初期化後にVagrantfileというファイルが出来ていることを確認する。  
これはrubyで書かれた設定ファイル。  
その後以下コマンドでvagrantを起動する。
```
$ vagrant up
```
仮想マシンの実態はvirtualboxで確認することができる。

### 状態の確認
```
$ vagrant status
```

### 一時停止/起動
#### 一時停止
```
$ vagrant suspend
```
#### 起動
```
$ vagrant resume
```
#### 再起動
Vagrantfileを変更した後に便利
```
$ vagrant reload
```

### virtual machineの削除
```
$ vagrant destroy
```

### boxの作成
```
// packageの作成
$ vagrant package

// packageを元にboxの作成(my_boxは今回作成するbox名)
$ vagrant box add my_box package.box

// boxが出来たことの確認
$ vagrant box list
centos7 (virtualbox, 0)
my_box  (virtualbox, 0)
```
