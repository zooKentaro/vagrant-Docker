# 仮想環境
### 仮想環境とは
仮想環境とは1台の物理サーバの中にあたかも もう1台端末があるかのように振る舞うことができる環境です。  
例えると仮想化技術を利用することで、Windowsマシン上でMacやLinuxなどのOSを動かすことも可能となります。  
この元々インストールされていた基本のソフトウェアを「**ホストOS**」、  
仮想化技術によりインストールされた基本ソフトウェアのことを「**ゲストOS**」と呼びます。
    
仮想環境を構築する方式は使用ソフトウェアの種類によって3種類に大別されます。  

- ##### ホスト型  
最も一般的な方式で別名「ホストOS型」と呼ばれています。  
ホスト型はホストOSと本来のハードウェアの上で仮想化ソフトウェアを実行させ、その上でゲストOSを運用する方式です。  
ホスト型のメリットは仮想化に必要なソフトウェアが扱いやすい点です。  
一般的なアプリケーションと同じ感覚で仮想化ソフトウェアをインストールでき、ちょっとした検証環境の構築には適しています。  
デメリットはゲストOSを運用する場合にホストOS側での処理も必要なことから処理速度が出にくい点です。  
そのため本番環境での使用には適しているとは言えません。  
ホスト型の主要なソフトウェアは**VMware Player**や**Oracle VM VirtualBox**です。  

<div style="text-align: center">
<img src="https://www.casleyconsulting.co.jp/wordpress/wp-content/uploads/sites/12/2017/05/container02.png" width="80%" height="80%">
</div>

- ##### ハイパーバイザー型
「ホスト型」でのデメリットである処理速度が出にくい点を改善するために、考案された仮想環境の構築方法です。  
専用の仮想化管理ソフトウェア「ハイパーバイザ」を導入し、その上で複数のゲストOSを動作させます。  
ホストOSがない分ホスト側の処理が不要となり、ホスト型と比べると処理速度の向上が期待できるのが魅力です。  
デメリットは「ホスト型」のように仮想化ソフトウェアを手軽にインストールすることはできず、仮想化環境を管理するための知識と技術が必要となります。  
ハイパーバイザー型の主要なソフトウェアは**VMware vSphere**や**KVM**、**Hyper-V**です。

<div style="text-align: center">
<img src="https://www.casleyconsulting.co.jp/wordpress/wp-content/uploads/sites/12/2017/05/container03.png" width="80%" height="80%">
</div>

- ##### コンテナ型
「ホスト型」や「ハイパーバイザ型」より比較的新しい仮想環境の構築方法です。  
コンテナ型の特徴としてホストマシンのOSはホストOSのみで仮想化ソフトウェアの上に「**コンテナ**」と呼ばれる領域が構築されます。  
上記2種の従来の仮想化技術では、仮想マシン上でゲストOSを起動する必要がありました。  
これに対しコンテナ型ではゲストOSを起動することなく、アプリケーション実行環境を構築することが可能です。  

<div style="text-align: center">
<img src="https://www.casleyconsulting.co.jp/wordpress/wp-content/uploads/sites/12/2017/05/container01.png" width="80%" height="80%">
</div>

### 仮想環境を使用して開発を行うメリット
仮想環境を構築することにより１台の端末を2台、3台...として振る舞うことができます。  
そのため物理マシンの購入数を最小限に抑えることが可能とります。

# Vagrant
### Vagrantとは
仮想マシンの構築や、どこでも同じ環境を再現できるように仮想マシンを管理するためのコマンドラインツールです。Widnwos、Mac、LinuxのOS上で動作します。  
Vagrant自体には仮想化機能はなく、VirtualBoxなどの仮想化ソフトウェアのフロントエンドとして機能し、仮想化ソフトウェアの操作を簡単なコマンドで代行してくれます。

### Vagrant環境構築の簡単な流れ

まず仮想化ソフトウェアであるVirtual Boxをダウンロードします。 [Virtual Box公式](https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)  
次にVagrantです。各OSごとに配布されているため自身の使用しているOSに合ったファイルをインストールしましょう。[Vagrant公式](https://www.vagrantup.com/)  

Macを使っている場合は以下コマンドで簡単にインストールすることが可能です。  
**`brew cask install vagrant`**  
**`brew cask install virtualbox`**  

インストールが完了したら問題なく使用できるか確認するために以下のコマンドを実行します。  
**`vagrant -v`**  

問題がなければ以下のように表示されます。  
**`Vagrant 2.2.5`**

***
####boxの作成
boxとは仮想環境を構築するために元となるOSが必要になります。
boxもコマンドを打つことで作成することができます。  
※boxを作成する際ディレクトリはどこでも良いです。  

vagrant box add box名  
例) **`vagrant box add centos/7`**  
コマンドを実行すると下記のような選択肢が表示されますので3を選択しましょう。  

1) hyperv  
2) libvirt  
3) virtualbox  
4) vmware_desktop  

Enter your choice: 3

***
#### vagrantの作業ディレクトリを用意する
vagrantの作業用ディレクトリを作成します。  
ディレクトリ名、格納場所は任意で問題ありません。  
boxの作成が完了したら今作成したフォルダ内で以下コマンドを実行します。  
**`vagrant init centos/7`**

***
####vagrant fileの編集
作成されたVagrantfileを開き以下2箇所に#でコメントアウトされているため削除してコメントインします。  
**`config.vm.network "forwarded_port", guest: 80, host: 8080`**  
**`config.vm.network "private_network", ip: "192.168.33.10"`**

***
####vagrantの起動
Vagrantファイルのあるディレクトリで以下コマンドを実行します。  
**`vagrant up`**

vagrantの起動が完了したら仮想環境にアクセスします。  
**`vagrant ssh`**

以下のようになっていれば接続成功です。  
`Last login: Mon Jul 29 06:17:10 2019 from 10.0.2.2`  
`[vagrant@localhost ~]$`

***
#### パッケージの導入
構築したい環境に合わせてパッケージを導入していきます。  
先に開発に必要なパッケージをインストールします。  
**`sudo yum -y groupinstall "development tools"`**  

apacheやnginx、Laravelなど...  
目的に合わせて設定を行っていきましょう  


# Docker
### Dockerとは
<img src="https://i2.wp.com/410gone.click/blog/wp-content/uploads/2018/02/horizontal.png?fit=297%2C77&ssl=1">  

Docker社が開発し、コンテナと呼ばれるOSレベルの仮想化環境を提供するオープンソースソフトウェアです。  
Dockerの特徴として**軽量**かつ**設定の再利用が可能**というものがります。  
「ホスト型」や「ハイパーバイザ型」は環境を動作させる場合は、目的以外のサービスを多数動作させる必要があります。  
それに比べDockerはコンテナを立ち上げた際に他のプロセスや他コンテナからは隔離され、固有の設定を持てるようになっています。必要なプロセスのみコンテナに入っているので処理速度が他と比べ高速です。  
また、一度作成したコンテナの設定を他コンテナへ適用させることが可能です。検証したいときやリソースを拡大したい時など、すぐに同じ環境設定が適用されたコンテナを準備することができます。

### Docker環境構築の簡単な流れ
Dockerのダウンロード手順は2種あります。  

- [公式サイト](https://hub.docker.com/editions/community/docker-ce-desktop-mac)でDockerアカウントを作成・ログインし、DockerHubからダウンロードする方法

- Homebrew経由でイントールする方法

今回は2つ目のHomebrew経由でインストールする方法で説明しようと思います。  
以下コマンドをターミナルに打つことでHomebrewにDockerをインストールできます。  
**`brew cask install docker`**  

インストールが完了したら、ちゃんと入っているか確認するために以下のコマンドを打ちましょう。  
**`docker --version`**

これだけでDockerを使うための準備はできました。  
後は構築したい環境に合わせて必要なイメージをカスタマイズします。  
自身でパッケージを準備しビルドする方法と、DockerHubに登録されているイメージを取得方法するがあります。  
どちらの方法でも導入は可能ですが、DockerHubからイメージを取得してそれをベースに改造する方法が一般的な利用方法とされています。  

# VagrantとDockerの違い
Vagrantは仮想マシンを動かす仮想化ソフトのラッパーツールで、  
DockerはOS・ミドルウェア・ファイルシステム全体をイメージという単位で取り扱い、まるごとやり取りできるツールです。  
仮想化の技術の種類としてVagrantは「ホスト型」でDockerは「コンテナ型」という違いがあります。  
処理速度の観点で言うと***Dockerとは*** で触れてましたが、「コンテナ型」であるDockerの方が高速です。  
またその他にもDockerのメリットとして以下の点があります。  

- OSの依存がなく導入が容易
- 案件ごとに異なる環境を構築できるため、特定のPC依存を回避
- ミドルウェア導入や新インフラ環境のテストが各自のPCで可能
- 言語やツールのバージョンアップテストが容易
- チームメンバー全員が各自のPCでデバッグ可能になる  

学習コストは多少かかるものの環境構築の手軽さ、処理速度の速度からDockerに軍配が上がっている模様です。(・・)／◇

***
参照元  
[Dockerのメリット・デメリット](https://www.casleyconsulting.co.jp/blog/engineer/257/)  
[kuguru](https://kuguru.jp/3544)  
[Vagrant + VirtualBoxでWindows上に開発環境をサクッと構築する](https://qiita.com/ozawan/items/160728f7c6b10c73b97e)  
[Dockerとは何かを入門者向けに解説！基本コマンドも](https://udemy.benesse.co.jp/development/web/docker.html)  
[Docker入門 #1 【Dockerとは】](https://qiita.com/wMETAw/items/b9bc643ded4b92bf6add)  
[この一冊で全部わかる サーバーの基本](https://www.amazon.co.jp/dp/B01DBQQ80A/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1)(きはし まさひろ著)

