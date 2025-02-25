# 第四回コマンド集

## 目次
1. [aptによるopenssh-serverのインストール](#pos1)
2. [scpによるファイル転送](#pos2)
3. [wgetでのファイルのダウンロード](#pos3)

<a id="pos1"></a>

## aptによるopenssh-serverのインストール

以下のコマンドを実行して、
openssh-serverパッケージをインストールします。

1. `sudo apt update`
2. `sudo apt upgrade`
3. `sudo apt install openssh-server`
4. `sudo apt show openssh-server`

今回は、最新のopenssh-serverパッケージをインストールします。

### `sudo apt update`

aptで管理されているインストール可能なパッケージ情報の一覧を最新版に更新します。

apt コマンドの実行には管理者(root)権限が必要です。  
Ubuntuでは管理者(root)ユーザでログインすることはできません。  
現在のユーザの状態でaptコマンドを実行するにはsudoコマンドを使用する必要があります。

以下のコマンドを実行して、apt内のパッケージ情報の一覧を最新版に更新してください。

```
sudo apt -y update
```

### `sudo apt upgrade`

`sudo apt update`では、インストール可能なパッケージ情報の一覧を最新にしただけです。  
今のまま、openssh-serverパッケージをインストール使用としても、現在Ubuntu内にインストールされている、依存関係にあるパッケージのバージョンが古いためにインストールに失敗してしまいます。  

以下のコマンドを実行して現在Ubuntuにインストールされているパッケージのバージョンを最新版にアップグレードしてください。

```
sudo apt -y upgrade
```

<details><summary>実行結果</summary>

更新情報がない場合

```
$ sudo apt upgrade
[sudo] password for mak:
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages were automatically installed and are no longer required:
  libllvm17t64 libwrap0 ncurses-term openssh-sftp-server
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
$
```

</details>

正常に処理が終了すれば、エラー(E:)は出力されません。  
エラーが出力されている場合には、出力内容を確認してください。  


### `sudo apt install openssh-server`

openssh-server パッケージをインストールします。
オプション「-y」はインストール時の確認を全てyesで処理します。

```
sudo apt -y install openssh-server
```

<details><summary>実行結果</summary>

```
$ sudo apt -y install openssh-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following package was automatically installed and is no longer required:
  libllvm17t64
Use 'sudo apt autoremove' to remove it.
Suggested packages:
  molly-guard monkeysphere ssh-askpass
The following NEW packages will be installed:
  openssh-server
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 509 kB of archives.
After this operation, 2151 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 openssh-server amd64 1:9.6p1-3ubuntu13.8 [509 kB]
Fetched 509 kB in 2s (216 kB/s)
Preconfiguring packages ...
Selecting previously unselected package openssh-server.
(Reading database ... 47203 files and directories currently installed.)
Preparing to unpack .../openssh-server_1%3a9.6p1-3ubuntu13.8_amd64.deb ...
Unpacking openssh-server (1:9.6p1-3ubuntu13.8) ...
Setting up openssh-server (1:9.6p1-3ubuntu13.8) ...

Creating config file /etc/ssh/sshd_config with new version
Creating SSH2 RSA key; this may take some time ...
3072 SHA256:1Ml2wjzQPDq+Zq3hGhlYOvpWXFZTXCeMO5RDMaa9qHM root@MAKvaioS14 (RSA)
Creating SSH2 ECDSA key; this may take some time ...
256 SHA256:G9IVKoOi9SFH386FtkUdkZSwAZjo+HBu8GmDrjGVQj4 root@MAKvaioS14 (ECDSA)
Creating SSH2 ED25519 key; this may take some time ...
256 SHA256:u1KPUkWL3r9OFVEmtoKcvYFH1w6t5468NsiYRVZ1560 root@MAKvaioS14 (ED25519)
Created symlink /etc/systemd/system/sockets.target.wants/ssh.socket → /usr/lib/systemd/system/ssh.socket.
Created symlink /etc/systemd/system/ssh.service.requires/ssh.socket → /usr/lib/systemd/system/ssh.socket.
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for ufw (0.36.2-6) ...
$
```

</details>


### `apt list --installed | grep "openssh"`

openssh-server パッケージがインストールされているか確認してください。

```
apt list --installed | grep "openssh"
```

<details><summary>実行結果</summary>

```
$ apt list --installed | grep "openssh"

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

openssh-client/noble-updates,noble-security,now 1:9.6p1-3ubuntu13.8 amd64 [installed,automatic]
openssh-server/noble-updates,noble-security,now 1:9.6p1-3ubuntu13.8 amd64 [installed]
openssh-sftp-server/noble-updates,noble-security,now 1:9.6p1-3ubuntu13.8 amd64 [installed,automatic]
$
```

</details>


<a id="pos2"></a>

## scpによるファイル転送

1. /etc/wsl.confの設定変更
2. WSL(Ubuntu)の再起動
3. sshサーバの起動(sshサービスの起動)
4. scpによるファイル転送
5. sshサーバの停止もしくはopenssh-serverパッケージのアンインストール

インストールしたopenssh-serverパッケージを使用して、sshサーバを起動させます。  
これにより、自身のパソコン(ローカルマシン)に対してssh通信が可能になります。  
scpはssh通信を利用してファイル転送・ダウンロードが可能です。

### /etc/wsl.confの設定変更

今回のsshサーバは、systemdと呼ばれるサービス管理プロセスによって管理されています。  しかし、WSLでのUbuntuは初期設定だとsystemdが起動していません。  
/etc/wsl.confの設定を変更してsystemdを起動可能な状態に変更します。

/etc/wsl.confをvimで開き、以下の様に変更してください。  
※rootユーザで管理されているファイルです。sudoコマンドも付けてvimを起動してください。

```
sudo vim /etc/wsl.conf
```

```
[boot]
#systemd=false
systemd=true
generateResolvConf = false
```

### WSL(Ubuntu)の再起動

**!!Ubuntu内の作業ではありません!!**

/etc/wsl.confの設定を反映する為にWSL(Ubuntu)を再起動する必要があります。

「Windows PowerShell」を起動して、以下のコマンドを入力してください。  
※PowerShellで検索すると出るはずです。

```
wsl.exe --shutdown
```

**!!Ubuntu内の作業ではありません!!**

### sshサーバの起動(sshサービスの起動)

WSL(Ubuntu)再起動後、sshサーバを起動します。
sshサーバを起動するには、systemdで管理されているsshサービスを起動する必要があります。
以下のコマンドを実行して、sshサービスを起動してください。

```
sudo systemctl start ssh.service
```

実行後、以下のコマンドでsshサービスが起動しているか、ssh用のポートが開いているか確認してください。  

```
sudo systemctl status ssh.service
```
```
netstat -a | grep ssh
```

### scpによるファイル転送

ローカルマシン上のsshサーバと通信して、ファイルを転送します。  
`scp 転送するファイル ユーザ名@ホスト名:転送先のパス`  
が文法です。ローカルマシン上にデータを転送するため、以下の形で情報を入力します。

- ユーザ名: Ubuntuに現在ログインしているユーザ名。
- ホスト名: `localhost`  ※localhostは自身のマシンを指します。
- 転送先のパス: `~/パス`  
パスは絶対パス、相対パスどちらでも指定できます。  
基本的にログイン先のユーザのホームディレクトリ配下に転送することが多いため、今回はチルダ(~)でホームディレクトリを指定して相対パスで指定してください。  

```
scp bbb.txt mak@localhost:~/ccc.txt
```

<details><summary>実行結果</summary>

```
$ ls
Basic_Command  aaa.txt  bbb.txt  work
$ scp bbb.txt mak@localhost:~/ccc.txt
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:u1KPUkWL3r9OFVEmtoKcvYFH1w6t5468NsiYRVZ1560.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
mak@localhost's password:
bbb.txt                                                                               100%    0     0.0KB/s   00:00
$ ls
Basic_Command  aaa.txt  bbb.txt  ccc.txt  work
```

</details>

### sshサーバの停止もしくはopenssh-serverパッケージのアンインストール

以下コマンドでsshサービスを終了して、sshサーバを停止してください。

```
sudo systemctl stop ssh.service
```

特に残しておきたい事情が無ければ、openssh-serverパッケージを削除してください。

```
sudo apt -y purge openssh-server
```

<details><summary>実行結果</summary>

```
$ sudo apt -y purge openssh-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libllvm17t64 libwrap0 ncurses-term openssh-sftp-server
Use 'sudo apt autoremove' to remove them.
The following packages will be REMOVED:
  openssh-server*
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 2151 kB disk space will be freed.
(Reading database ... 47227 files and directories currently installed.)
Removing openssh-server (1:9.6p1-3ubuntu13.8) ...
Processing triggers for man-db (2.12.0-4build2) ...
(Reading database ... 47208 files and directories currently installed.)
Purging configuration files for openssh-server (1:9.6p1-3ubuntu13.8) ...
Processing triggers for ufw (0.36.2-6) ...
$
```

</details>

<a id="pos3"></a>

## wgetでのファイルのダウンロード

自身のGoogleドライブ等に存在するファイルをURL指定でダウンロードしてみてください。

```
wget ダウンロード対象のURL
```