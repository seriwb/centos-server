# dev-server

[CentOS 7の仮想OS](https://atlas.hashicorp.com/centos/boxes/7)上で、各種動作確認環境を構築するプロジェクトです。


## 実行要件

予め以下のソフトウェアとVagrantのプラグインを、ホストOSにインストールしてから実行してください。

### ソフトウェア

- Vagrant 2.0.0+
- VirtualBox 5.1.28+

### Vagrantプラグイン

- vagrant-vbguest
<!-- - vagrant-reload -->

Vagrantプラグインのインストール手順は以下になります。

    $ vagrant plugin install <プラグイン名>


### for Windows Users

ホストOSにWindowsを利用する場合は、追加で以下の作業を行ってください。

#### RSyncのインストール

RSyncをインストールしてください。
[Chocolatey](https://chocolatey.org/)や[Scoop](http://scoop.sh/)でのインストールがお奨めです。


#### 環境変数の追加

環境変数に以下を追加してください。

- ```CYGWIN```に```nodosfilewarning```を追加（スペース区切り）

また、インストールしたRSyncのパスをPATHに追加してください。
（以下はChocolateyの場合の例）

- ```PATH```に```C:\ProgramData\chocolatey\lib```を追加（;区切り）


## 実行手順

GitHub Enterpriseで利用しているSSHの秘密鍵をssh-addで追加後、
cloneしたディレクトリ直下で、以下のコマンドを実行してください。

    $ vagrant up

rsyncの自動同期が上手くいかない場合は、ゲストOS起動後に以下のコマンドをホストOSで実行してください。

    $ vagrant rsync-auto


## 動作説明

Provisioningで以下のAnsible Playbookが実行されます。
※ホストOSにAnsibleは不要です。

    ansible/local.yml

※開発に利用するリポジトリは、repositoriesディレクトリに配置する想定で作っています。


----
# Ansible Playbook

ansibleディレクトリ配下のPlaybookで、環境構築を実施します。


## 実行要求

Vagrant以外から実行する場合は、実行環境にAnsible 2.3.0+ をインストールしてください。


## 開発環境構築の手動実行方法

開発環境のサーバにansibleディレクトリをコピーし、当該ディレクトリ上で以下のコマンドを実行してください。

    ansible-playbook -i local local.yml

※SSHのagent forwardingを使ってgit cloneしているので、GitHubの秘密鍵がssh-addで登録されている必要があります。  
vagrant sshしてからansible-playbookコマンドを叩く場合も、ホストOS側で事前にssh-addの実行をお願いします。


## Roles

以下のロールを用意しています。

### 言語

- Go
- Java
- Node.js
- Ruby


### ビルド関連

- Ant
- Gradle
- Jenkins


### ミドルウェア

- Apache
- Docker
- gRPC


### DB

- PostgreSQL
