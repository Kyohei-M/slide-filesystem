name: inverse
layout: true
class: center, middle, inverse

---
# ファイルとリンク

---
layout: false
class: center, middle

![twitter](twitter.png)

---
class: center, middle, inverse

# うーん…？

---
## 対象者

* Windows、Linux使ってる
* ファイルわからない
* リンクわからない

<br/>

## 目標

* ファイル少しわかるようになる
* リンク少しわかるようになる

---
class: center, middle, inverse
# ファイル？

---
## ファイルシステム

* OS内でディスク管理を行う機能
* ディスクのパーティション毎にフォーマットされる
* ファイルのメタデータを管理

---
## ファイルシステムの種類

|&nbsp;ファイルシステム名&nbsp;|&nbsp;最大ファイルサイズ&nbsp;|&nbsp;最大ボリュームサイズ&nbsp;|主な利用先|
|:--:|:--:|:--:|--|
|FAT32|4GiB|2TiB|Windows(32bit)|
|ext4|16TiB|1EiB|Linux|
|NTFS|16EiB|16EiB|Windows|
|XFS|8EiB|8EiB|Linux|
|ISO 9660|-|-|CD-ROM|
|UDF|-|-|DVD-ROM|

---
## 分散ファイルシステム

* 他のサーバやストレージ上に構築されているファイルシステムをネットワーク経由で利用
* オンラインストレージを構築後、マウント(Linux)orネットワーク共有(Windows)を行う

---
class: center, middle, inverse
## Linux/UNIXのファイル管理

---
## ファイルのメタデータ

* inodeを使用
* ext4ではサイズ固定、xfsではサイズ可変
* 特定ファイルやディレクトリのinode情報を見る場合はstatコマンド

```console 
$ stat /home/user/testfile

```

---
## ディレクトリエントリ

* ディレクトリ自体に記される情報
* inode番号とファイル名orディレクトリ名を含む
* lsコマンドで確認

```console
$ ls -i | cat

```

---
## リンク

ファイルやディレクトリに対して、別名でアクセスする機能  
Linuxには2種類のリンクが存在

* シンボリックリンク
* ハードリンク

---
## シンボリックリンク

* ファイル、ディレクトリに対して作成可能
* 元ファイルやディレクトリとは異なるinode番号が付与される
* 元ファイルやディレクトリが削除されると、リンク切れ状態になる

---
## ハードリンク

* ファイルに対してのみ作成可能
* 同じパーティション内のファイルのみ
* 元ファイルと同じinode番号が付与される
* 元ファイルが消されてもアクセス可能

---
## 2つのリンクの挙動の違い

```console


```

---
class: center, middle, inverse
## Windowsのファイル管理

---
## NTFS

* New Technology File System
* Microsoft製のプロプライエタリなファイルシステム
* ファイルやディレクトリのメタデータを格納するためのMFTを用意

<br/>
<br/>

## MFT

* Master File Table
* ファイルレコード、ファイル属性の列を持つRDB
* パーティション毎に隠しファイル($MFT)として作られる

---
## リンク

Windowsの場合は3種類のリンクが存在

* シンボリックリンク
* ジャンクション
* ハードリンク

<br/>

|機能|&nbsp;シンボリックリンク&nbsp;|&nbsp;ジャンクション&nbsp;|&nbsp;ハードリンク&nbsp;|
|--|:--:|:--:|:--:|
|作成に必要な権限|管理者|一般ユーザー|管理者|
|ファイルへのリンク|〇|✖|〇|
|ディレクトリへのリンク|〇|〇|✖|

---
## リンク

```console
C:\temp> echo This is sample. > origin.txt
C:\temp> mkdir OriginDir

C:\temp> mklink symlink.txt origin.txt
symlink.txt <<===>> origin.txt のシンボリック リンクが作成されました

C:\temp> mklink /d SymlinkDir OriginDir
SymlinkDir <<===>> OriginDir のシンボリック リンクが作成されました

C:\temp> mklink /j JunctionDir OriginDir
JunctionDir <<===>> OriginDir のジャンクションが作成されました

C:\temp> mklink /h hardlink.txt origin.txt
hardlink.txt <<===>> origin.txt のハードリンクが作成されました

```

---
## リンク

```console
C:\temp> dir
 ドライブ C のボリューム ラベルは Windows です
 ボリューム シリアル番号は 4614-0FD7 です

 C:\temp のディレクトリ

2018/11/08  01:22    <DIR>          .
2018/11/08  01:22    <DIR>          ..
2018/11/08  01:16                18 hardlink.txt
2018/11/08  01:21    <JUNCTION>     JunctionDir [C:\temp\OriginDir]
2018/11/08  01:16                18 origin.txt
2018/11/08  01:18    <SYMLINKD>     symlink.txt [origin.txt]
2018/11/08  01:20    <SYMLINKD>     SymlinkDir [OriginDir]

```

---
class: center, middle, inverse
## ファイル少しわかったかな？

---
## 参考  

書籍  
[インフラエンジニアの教科書2](https://www.amazon.co.jp/%E3%82%A4%E3%83%B3%E3%83%95%E3%83%A9%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E3%81%AE%E6%95%99%E7%A7%91%E6%9B%B82-%E3%82%B9%E3%82%AD%E3%83%AB%E3%82%A2%E3%83%83%E3%83%97%E3%81%AB%E5%8A%B9%E3%81%8F%E6%8A%80%E8%A1%93%E3%81%A8%E7%9F%A5%E8%AD%98-%E4%BD%90%E9%87%8E-%E8%A3%95/dp/4863541864)

Wikipedia  
[ファイルシステム](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0#Windows%E3%81%AE%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)  
[NTFS](https://en.wikipedia.org/wiki/NTFS)
