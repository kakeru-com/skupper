skupperを用いてマルチクラウドを行います。
*前提条件としてGIP、floatungIPが必要です


1.仮想マシンやサーバにkubernetes環境を構築する

2.curl -L https://github.com/skupperproject/skupper/releases/download/1.5.1/skupper-cli-1.5.1-linux-amd64.tgz -o skupper-cli.tgz

tar zxvf skupper-cli.tgz

sudo mv skupper /usr/local/bin/

sudo chmod +x /usr/local/bin/skupper

sudo chmod +x skupper
