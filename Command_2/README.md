# 第二回コマンド集

**実行結果はコピーしないでください。**

## 目次
1. [pwd](#pos1)
2. [ls](#pos2)
3. [cd](#pos3)
4. [find](#pos4)
5. [mkdir](#pos5)
6. [cat](#pos6)
7. [echo](#pos7)
8. [rm](#pos8)
9. [rmdir](#pos9)
10. [man](#pos10)

<a id="pos1"></a>

## pwd

現在の作業用(カレント)ディレクトリを表示

```
pwd
```
相対パスで何かしらを指定する場合、「pwd」で表示されたディレクトリが基準となります。

<details><summary>実行結果</summary>

```
$ pwd
/home/mak
```

</details>

<a id="pos2"></a>

## ls

指定したディレクトリ内のディレクトリ・ファイルの一覧を表示します。

何も指定していない場合には、現在のディレクトリ内のディレクトリ・ファイルの一覧を表示します。


```
ls
```

<details><summary>実行結果</summary>

```
$ ls
Basic_Command 
```

</details>

ディレクトリ・ファイルのパスを指定すると、そのディレクトリ内の一覧を確認できます。

```
ls ./Basic_Command/
```

<details><summary>実行結果</summary>

```
$ ls ./Basic_Command/
README.md  README.md:Zone.Identifier
```

</details>


コマンドには様々なオプションが指定できる場合があります。

「-a」であれば、隠しファイル(ドット(.)が最初につくファイル)が表示できます。

```
ls -a
```

「-l」であれば、ディレクトリ・ファイルの詳細な情報が確認できます。

```
ls -l
```

<details><summary>実行結果</summary>

```
$ ls -a
.              .dotnet      .sudo_as_admin_successful
..             .gitconfig   .viminfo
.bash_history  .gnupg       .vscode-remote-containers
.bash_logout   .landscape   .vscode-server
.bashrc        .lesshst     .wget-hsts
.cache         .motd_shown  Basic_Command
.docker        .profile     work
```

```
$ ls -l
drwxr-xr-x 2 mak mak 4096 Feb 22 16:40 Basic_Command
drwxr-xr-x 3 mak mak 4096 Jan 10 20:39 work
```

```
ls -la
total 80
drwxr-x--- 11 mak  mak  4096 Feb 22 17:55 .
drwxr-xr-x  3 root root 4096 Jan  8 20:54 ..
-rw-------  1 mak  mak  5755 Feb 22 16:52 .bash_history
-rw-r--r--  1 mak  mak   220 Jan  8 20:54 .bash_logout
-rw-r--r--  1 mak  mak  3771 Jan  8 20:54 .bashrc
drwx------  3 mak  mak  4096 Jan  9 20:07 .cache
drwx------  3 mak  mak  4096 Jan 10 20:28 .docker
drwxr-xr-x  3 mak  mak  4096 Jan  9 21:00 .dotnet
-rw-r--r--  1 mak  mak    56 Jan  8 20:58 .gitconfig
drwx------  3 mak  mak  4096 Jan 10 22:26 .gnupg
drwxr-xr-x  2 mak  mak  4096 Jan  9 19:57 .landscape
-rw-------  1 mak  mak    36 Feb 22 17:55 .lesshst
-rw-r--r--  1 mak  mak     0 Feb 22 15:58 .motd_shown
-rw-r--r--  1 mak  mak   807 Jan  8 20:54 .profile
-rw-r--r--  1 mak  mak     0 Jan  8 20:56 .sudo_as_admin_successful
-rw-------  1 mak  mak  1187 Jan 10 21:05 .viminfo
drwxr-xr-x  3 mak  mak  4096 Jan  9 20:07 .vscode-remote-containers
drwxr-xr-x  5 mak  mak  4096 Jan  9 20:07 .vscode-server
-rw-r--r--  1 mak  mak   164 Jan  8 21:11 .wget-hsts
drwxr-xr-x  2 mak  mak  4096 Feb 22 16:40 Basic_C
```

</details>

<a id="pos3"></a>

## cd

作業用ディレクトリを変更

指定したディレクトリを作業用ディレクトリとして、現在のディレクトリから移動します。


```
cd Basic_Command
```

コマンド実行後、「pwd」を実行することで、作業用ディレクトリが変更されていることを確認できます。  
※シェルによっては現在のディレクトリ名(またはパス)が確認できます。

<details><summary>実行結果</summary>

```
~$ cd Basic_Command/
~/Basic_Command$
※一度スペースを入力
~/Basic_Command$ pwd
/home/mak/Basic_Command
```

</details>

「cd」のみの場合、ログインしているユーザのホームディレクトリを作業用ディレクトリとして移動します。

```
cd
```

<details><summary>実行結果</summary>

```
~/Basic_Command$ cd
~$
```

</details>


<a id="pos4"></a>

## find

条件に合致するファイルを探索します。  
以下のコマンドであれば、「Basic_Command」ディレクトリ内の先頭が「READ」であるファイルを探索して表示します。

```
$ find Basic_Command -name "READ*"
```

<details><summary>実行結果</summary>

```
$ find Basic_Command -name "READ*"
Basic_Command/README.md:Zone.Identifier
Basic_Command/README.
$ 
```

</details>

<a id="pos5"></a>

## mkdir

指定したディレクトリを作成します。

```
mkdir Basic_Command/Example
```

<details><summary>実行結果</summary>

```
$ mkdir Basic_Command/Example
$ ls Basic_Command/
Example  README.md
$
```

</details>

<a id="pos6"></a>

## cat

指定したファイルの内容を表示します。

```
cat Basic_Command/README.md
```

<details><summary>実行結果</summary>

```
$ cat Basic_Command/README.md
# Basic_Command
Linux基礎コマンド勉強会

---

```

</details>

<a id="pos7"></a>

## echo

指定したテキストの一覧を表示。
以下コマンドであれば、環境変数「HOME」に設定されているディレクトリ名を表示します。

```
echo $HOME
```

<details><summary>実行結果</summary>

```
$ echo $HOME
/home/mak
```

</details>

また、以下の使い方ではファイルを作成します。

```
$ echo "Hello World." > Basic_Command/Example/hello.txt
```

<details><summary>実行結果</summary>

```
$ cat Basic_Command/Example/hello.txt
Hello World.
```

</details>

<a id="pos8"></a>

## rm

指定したファイルを削除します。

```
rm Basic_Command/Example/hello.txt
```

<details><summary>実行結果</summary>

```
$ rm Basic_Command/Example/hello.txt
$ ls Basic_Command/Example/
$
```

</details>

<a id="pos9"></a>

## rmdir

指定したディレクトリを削除します。
しかし、空のディレクトリのみ削除可能です。  
何かしら入っているディレクトリは削除できません。

```
rmdir Basic_Command/Example/
```

<details><summary>実行結果</summary>

```
$ rmdir Basic_Command/Example/
$ rmdir Basic_Command/Data/
rmdir: failed to remove 'Basic_Command/Data/': Directory not empty
```

</details>

<a id="pos10"></a>

## man

指定した文字列(ディレクトリ名、ファイル名、コマンド名)に対応するオンラインマニュアルを表示

```
man ls
```

基本的に、括弧[]で囲まれている箇所は任意のオプションであり、囲まれていない箇所は必須の入力値です。

<details><summary>実行結果</summary>

```
$ man ls

---

LS(1)                       User Commands                      LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION

```

</details>