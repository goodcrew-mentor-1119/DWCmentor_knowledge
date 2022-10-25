# cloud9で「tmux: invalid LC_ALL, LC_CTYPE or LANG」エラー
セッションマネージャーでEC2にログインすることで対応します。

cloud9のEC2インスタンスにセッションマネージャーからログインする方法は下記です。
　
```
cloud9のEC2インスタンスにセッションマネージャーからログインする方法

■AWS Systems Manager の設定
ホスト管理の設定

AWS Systems Manager コンソールに移動し、ナビゲーションペインで[高速アップ(クイック セットアップ)] を選択します。
[クイック セットアップ]ページで、 [作成]を選択します。

構成タイプについては、[ホスト管理]に移動し、[作成]を選択します。
(リーションはcloud9があるリージョンを選択)

[ Customize Host Management configuration options ]の
[ Targets]セクションで、 [ Manual(手動) ] を選択します。

アクセスする EC2 インスタンスを選択し、[作成]を選択します。

■セキュリティーグループの設定

・セキュリティーグループの設定
cloud9のEC2インスタンスのセキュリティーグループに、
インバウンドルール ssh 0.0.0.0/0 を設定

■接続方法
Amazon EC2 コンソールのナビゲーションペインで、 [インスタンス] を選択し、接続するインスタンスを選択します。
[接続]を選択します。
[ Connect to your instance ] ペインの[ Connection method ] で[EC2 Instance Connect ]を選択し、

ユーザー名を ec2-user に変更

[接続]ボタンをクリックするとセッションマネージャー画面が起動し、EC2のターミナル画面が開きます。
```
ログインすると下記のエラー表示がされます。
```
locale
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_MESSAGES to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
```
ログインしたターミナルでの作業

インストール済みのロケールファイルを確認
```
locale -a 
```
ディフォルトで使用している「en_US.UTF-8」が見当たらない（インストールされていない？、削除されている？）

ロケールファイルをインストール

```
sudo yum install  glibc-langpack-ja
sudo yum install  glibc-langpack-en
```

locale -a でインストールされているファイルを確認

ja_JP.utf8が存在するのでディフォルトロケールをja_JP.utf8に設定

ロケールの設定を変更
```
sudo vi /etc/locale.conf
```
/etc/locale.confの内容
```
LANG=ja_JP.utf8 
```

## 最後にcloud9のEC2インスタンスのセキュリティーグループのインバウンドルールから ssh 0.0.0.0/0 を削除を忘れずに行ってください。


