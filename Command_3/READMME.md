# 第三回コマンド集

## 目次

1. [vimの起動から編集内容の保存](#pos1)
2. [テキストのコピー](#pos2)
3. [一つ前の作業を巻き戻し](#pos3)
4. [ファイル内検索](#pos4)
5. [置換](#pos5)


<a id="pos1"></a>

## vimの起動から編集内容の保存

### 1. vimの起動  

```
vim Basic_Command/DATA/learn1.txt
```

learn1.txt のファイルを新規作成すると、以下の実行結果の様に
テキストエディタが起動されます。

<details><summary>実行結果</summary>

```

~                                                                 ~                                                                 "Basic_Command/DATA/learn1.txt" [New]  
```

</details>

### 2. 「ノーマルモード」から「挿入モード」への移行

vim起動時は「ノーマルモード」であり、テキストは入力できません。
以下のコマンドのいずれかを入力して。「挿入モード」へと移行する必要があります。

|コマンド|説明|
|---|---|
|i|現在のカーソル位置の左側から文章を挿入します。|
|a|現在のカーソル位置の右側から文章を挿入します。|
|o|現在の行の一行下から、改行して文章を挿入します。|
|I(shift + i)|行頭で「挿入モード」|
|A(shift + a)|行末で「挿入モード」|
|O(shift + o)|現在の行の一行上から、改行して文章を挿入します。|

最初は「i」を押下して、「挿入モード」へ移行してください。
エディタ下部に「-- INSERT --」が表示されていれば「挿入モード」に移行しており、テキストが入力できます。

<details><summary>実行結果</summary>

「i」を押下後

```

~
~
-- INSERT --     
```

</details>

### 3. テキストの入力

エディタ下部に「-- INSERT --」が表示されていれば「挿入モード」に移行しており、テキストが入力できます。  
何かしらのテキストを入力してください。

### 4. 「挿入モード」から「ノーマルモード」への移行

「挿入モード」から「ノーマルモード」へ移行します。
移行には、「Esc」ボタンを押下してください。  

エディタ下部に「-- INSERT --」が表示されていなければ、
「ノーマルモード」へと移行しています。

<details><summary>実行結果</summary>

「Esc」を押下後

```
This is the learning test.
~
~

```

</details>

### 5. 編集内容の保存(コマンドモード)

編集した内容を保存して、テキストエディタを終了します。

以下のコマンドの入力で保存後、テキストエディタが終了します。

```
:wq
```

<details><summary>実行結果</summary>

「Esc」を押下後

```
This is the learning test.
~
~
:wq  
```

「Enter」を押下後、テキストエディタが終了します。

</details>

### 6. 作成したファイルの内容の確認

ファイルが新規に作成されており、編集した内容が保存されているか確認してください。

```
cat Basic_Command/DATA/learn1.txt
```

<details><summary>実行結果</summary>

「Esc」を押下後

```
$ vim Basic_Command/DATA/learn1.txt
$ cat Basic_Command/DATA/learn1.txt
This is the learning test.
$ 
```

</details>

<a id="pos2"></a>

## テキストのコピー

「ノーマルモード」時に行単位でのコピー・ペーストが可能です。

「yy」で現在のカーソルが位置する行をヤンク(コピー)します。

```
yy
```

その後、「p」により、下の行に文章がペーストされます。

```
p
```

<details><summary>実行結果</summary>

```
This is the learning test.
This is the learning test. 
```

</details>

<a id="pos3"></a>

## 一つ前の作業を巻き戻し

「ノーマルモード」時に一つ前に行った作業を巻き戻すことが可能です。
「u」により作業を巻き戻せます。

```
u
```

<details><summary>実行結果</summary>

```
This is the learning test.
~
~
~
1 line less; before #1  11:25:06  
```

</details>


<a id="pos4"></a>

## ファイル内検索

「ノーマルモード」から、ファイル内の内容を指定したパターンと一致するか検索することが可能です。

### 検索するファイル

今回は以下の文書の中身を検索します。

```
Basic_Command/DATA/User/systemd.txt
```

各々の方法でsystmed.txtをテキストエディタで開いてください。

### 検索するパターンを指定

「/パターン」の要領で検索します。
今回であれば、"Systemd"を例に検索します。

```
/Systemd
```

<details><summary>実行結果</summary>

```
Systemd also introduces a robust logging system through its
component, journald. Instead of relying solely on traditional
log files like /var/log/messages, journald collects and
stores logs in a binary format, accessible via the journalctl
command. This allows for detailed filtering by time, service,
or priority, making troubleshooting more efficient. Administrators
/Systemd★              
```

</details>

### 検索対象が複数の場合

systmed.txtには、"Systemd"が3箇所あります。

次の検索対象への移動には、「n」を押してください。

|コマンド|説明|
|---|---|
|n|次(下)の検索結果に移動
|N(shitft + n)|前(上)の検索結果に移動|

<a id="pos5"></a>

## 置換

「ノーマルモード」から、文字を置換することが可能です。

### 検索するファイル

今回は以下の文書の中身を置換します。

```
Basic_Command/DATA/User/systemd.txt
```

各々の方法でsystmed.txtをテキストエディタで開いてください。

### 検索するパターンを指定

「:%s/置換前の文書/置換後の文書」
で文書を置換できます。  
しかし、上記の場合では1行の中で、一番最初に合致した部分しか置換されません。  
「:%s/置換前の文書/置換後の文書」で全て置換できます。

ここの例では、"Systemd"を"Learning"に置換します。

```
:%s/Systemd/Learning/g
```

<details><summary>実行結果</summary>

ファイル保存後、中身を確認すると置換されていることが確認できます。

```
■置換前
$ grep "Systemd" Basic_Command/DATA/User/systemd.txt
Systemd is a system and service manager for Linux operating
Systemd also introduces a robust logging system through its
Systemd extends beyond basic service management with tools
■ファイルの編集
$ vim Basic_Command/DATA/User/systemd.txt
■置換後
$ grep "Systemd" Basic_Command/DATA/User/systemd.txt
$ grep "Learning" Basic_Command/DATA/User/systemd.txt
Learning is a system and service manager for Linux operating
Learning also introduces a robust logging system through its
Learning extends beyond basic service management with tools
```

</details>
