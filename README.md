# skupperでマルチクラウド
今回はskupperを用いて地理的に離れた2つのリージョンをskupperでpublic-public接続を行う。

# 前提条件
*前提条件としてkubernetes環境(構築手順等は割愛)、GIP、floatungIPが必要

# 1.skupperインストール環境を構築
本ケースではcls1を仮想マシン上のネームスペース、cls2をサーバ側のネームスペースとしてskupper接続を行う
## cls1
`kubectl create namespace cls1`

`kubectl config set-context --current --namespace cls1`

## cls2
`kubectl create namespace cls2`

`kubectl config set-context --current --namespace cls2`

#  2.skupperをdownload

`curl -L https://github.com/skupperproject/skupper/releases/download/1.5.1/skupper-cli-1.5.1-linux-amd64.tgz -o skupper-cli.tgz
tar zxvf skupper-cli.tgz`

### skupperをバイナリに移動
`sudo mv skupper /usr/local/bin/`

### skupperのアクセス権を変更
`sudo chmod +x /usr/local/bin/skupper`


# 2.skupper初期化
## cls1
`skupper init --ingress nodeport --ingress-host cls1のマスターノードのIP or LB --console-user AAA --console-password AAA`
## cls2
`skupper init --ingress nodeport --ingress-host cls2のマスターノードのIP or LB --console-user CCC --console-password CCC`

# 3.skupper接続
cls1を接続先としてトークンの作成を行う。なお、skupperは双方向通信のためどの方向からでもリンクさえ張れれば通信が可能になる。
## cls1
`skupper token create cls1.token`

現在のディレクトリ上にトークンが作成される。
## cls1
`skupper link create cls1.token`
しばらく経てば
`user2@server: skupper link status

Links created from this site:

	 Link1 is connected

Current links from other sites that are connected:

	 There are no connected links`
  と表示される。

# 4.skupperコンソール画面にアクセス

  `user@VM:~# skupper status
Skupper is enabled for namespace "tyugoku". It is connected to 1 other sites (1 indirectly). It has 1 exposed service.
The site console url is:  https://skupper init時のアドレス:ポート番号
The credentials for internal console-auth mode are held in secret: 'skupper-console-users'`

と表示されるのでこのURLにアクセスすると

<img width="1404" alt="スクリーンショット 2024-02-09 18 27 40" src="https://github.com/kakeru-com/skupper/assets/135799071/08ff60a0-7e58-45ce-8283-bf98834719e5">


無事リンクが接続される。これでskupperによるリージョン間接続が容易に実現できる。
