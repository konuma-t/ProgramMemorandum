# CORESERVERにgitをインストールする

---  

下記内容は2021年1月1日時点での状況です。

GMOのCORESEVERにインストールされているはgitはバージョンが古いのでアップデートをしたかった。  
参考サイトの手順で進めたが内容が現状と違っていて、そのままではうまくいかなかった。  
忘れないうちに備忘録として残す。  
文末の参考サイトを先に読んでから、下記作業を進める。  

---  

1. CORESERVERで[SSH接続IP許可]、登録情報の確認
1. gitの最新バージョンを確認
1. CORESERVERにSSHで接続
1. CORESERVER側でインストールしたgitの確認
1. gitの最新バージョンのtarballファイルを入手
1. tarballファイルの解凍
1. ソースコードファイルのインストール
    1. ./configure
    1. gmake
    1. gmake install
1. インストール後の確認
1. PATHの設定
---
#### 1. CORESERVERで[SSH接続IP許可]、登録情報の確認  
CORESERVERはセキュリティのため、SSH接続するためには接続する端末からIP登録が必要。また、接続するための情報も確認する。  
**手順[^1]**  
CORESERVERのダッシュボードにログイン後  
* [サイト設定] > [ツール/セキュリティー] > [SSH接続IP許可]  
『SSH接続のIPを許可する』
* [サイト設定] > [FTP設定]  
『ホスト』  
『アカウント』  
『パスワード』  

---

#### 2. gitの最新バージョンを確認  
インストールするためのGitの最新バージョンを公式サイトで確認  
**手順**  
git([https://git-scm.com/](https://git-scm.com/))にアクセスして、バージョン名の『\*.\*.\*.\*』を確認(2021年1月1日時点では『2.30.0』)  

---  

#### 3. CORESERVERにSSHで接続  
CORESERVERに端末のIP登録が完了したら、ログインする。  
**手順[^1]**  
参考サイトを参照  

---  

#### 4. CORESERVER側でインストールしたgitの確認
CORESERVERにログインしたら、インストール済みのgitの情報を確認する。  
**手順**  
ホームディレクトリでgitコマンドが実行できる事の確認
```Shell:git実行の確認
git
```
gitのバージョンの確認(2021年1月1日時点では『1.8.3.1』)
```Shell:gitのバージョンの確認
git --version
```
gitの実行ファイルのPATHの確認(2021年1月1日時点では『/bin/git』)
```Shell:gitの保存の確認
which git
```



---
#### 5. gitの最新バージョンのtarballファイルを入手
ネットから『git-\*.\*.\*.\*.tar.gz』を入手する。  
**手順[^2]**  
ネット上の情報ではCORESERVERにSSHでログインして、
```Shell:tar.gz入手
wget http://git-core.googlecode.com/files/git-*.*.*.*.tar.gz
```
で入手できると書いてあるが、404: Not Foundエラーで入手できなかった。  
そのため『mirrors.kernel.org』から入手した。
[https://mirrors.kernel.org/pub/software/scm/git/](https://mirrors.kernel.org/pub/software/scm/git/)
から最新バージョンの『git-\*.\*.\*.\*.tar.gz』があるのを確認し、下記を実行する。
```Shell:tar.gzを入手
wget https://mirrors.kernel.org/pub/software/scm/git/git-*.*.*.*.tar.gz
```  
git公式サイトのDownloadsページ内の『Older releases』からもミラーサイトに行ける。

---
#### 6. tarballファイルの解凍
入手したtarballファイルを解凍する。  
**手順[^2]**  
```Shell:tar.gzを解凍する。
tar xvzf git-*.*.*.*.tar.gz
```
『git-\*.\*.\*.\*』ディレクトリが作成される。  

---
#### 7. ソースコードファイルのインストール  
『git-\*.\*.\*.\*』ディレクトリに移動後、３つのコマンドを実行する。  
**手順[^2]**  
『git-\*.\*.\*.\*』ディレクトリに移動する。  
```Shell:gitディレクトリに移動する。
cd git-*.*.*.*.tar.gz
```
ディレクトリに移動後、下記の３つのコマンドを実行する。  
##### 7-1. ./configure
configureはインストールに必要な環境を調べて『MakeFile』を作るためのスクリプトファイル。
ホームディレクトリにlocalディレクトリを作り、その中にインストールする。  
**手順[^2][^3]**  
```Shell:configureでMakeFileを作成する。
./configure --prefix=$HOME/local
```
##### 7-2. gmake
『MakeFile』からアプリのコンパイルを行う。  
**手順[^2][^3]**  
```Shell:MakeFileからアプリのコンパイル。
gmake
```
##### 7-3. gmake install
コンパイルされたアプリをインストールする。  
**手順[^2][^3]**  
```Shell:コンパイルされたアプリのインストール。
gmake install
```
---
#### 8. インストール後の確認  
今インストールしたgitの情報を確認する。  
**手順**  
ホームディレクトリから見て『/local/bin』に移動し、インストールしたgitの実行ファイルがある事を確認する。
```Shell:git実行ファイルの確認。
cd 
cd local/bin
ls -la
```

gitコマンドが実行できる事の確認
```Shell:git実行の確認
./git
```
gitのバージョンの確認(上記内容の場合『2.30.0』)
```Shell:gitのバージョンの確認
./git --version
```
gitの実行ファイルのPATHの確認(上記内容の場合『./git』)
```Shell:gitの保存の確認
which ./git
```
---  
#### 9. PATHの設定
PATHは複数のファイルに設定が書かれていて、設定ファイルの読み込み順番により優先順位が決まる。[^4]  
CORESERVER側でインストールされたgitよりも今回インストールしたgitのPATHが優先されるように設定する。  
**手順[^2]**  
ホームディレクトリにある『.bashrc』にPATH設定を追加する。  

参考サイトにある
```
export PATH=$PATH:/virtual/youraccount/local/bin
```
はCORESERVER側にgitがインストールされていない場合。  
個人のホームディレクトリの優先順位が低いので順番を変更する。
```
export PATH=${HOME}/local/bin:$PATH
```
\$PATHは環境変数で、既に設定されているPATH。  
\$HOMEは環境変数で、中身はログインユーザーのホームディレクトリ。(今回の場合は、『/virtual/youraccount』)  

『.bashrc』の内容を反映させる。  
```
source .bashrc
```

ホームディレクトリのgitコマンドでバージョンを確認する。(上記内容の場合『2.30.0』)  
```
git --version
```

---
### 参考サイト
[^1]:『Webエンジニアの仕事見聞録』』  
コアサーバーでSSHやsftp接続を行う方法  
[https://engineer-milione.com/create/coreserver-ssh.html](https://engineer-milione.com/create/coreserver-ssh.html)  
 [^2]:『WEBマスターの知恵ブログ』  
CORESERVERにgitをインストールする方法  
[https://webmaster.chielog.com/git/156.html](https://webmaster.chielog.com/git/156.html)  
[^3]:『Qiita@chihiro』  
configure, make, make install とは何か  
[https://qiita.com/chihiro/items/f270744d7e09c58a50a5](https://qiita.com/chihiro/items/f270744d7e09c58a50a5)  
[^4]:『Qiita@yunzeroin』  
[Linux]環境変数の読み込み順番  
[https://qiita.com/yunzeroin/items/480a3a677f78a57ac52f](https://qiita.com/yunzeroin/items/480a3a677f78a57ac52f)  