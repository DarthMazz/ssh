# ssh

## Azure に VM を作成して SSH で接続する(Linux)

### Azure に VM を作成する
リソースの作成 > Compute > 仮想マシン

* プロジェクトの詳細
  * リソースグループ:ssh-test-ubuntu1804
  * 仮想マシン名:ssh-test-ubuntu1804
  * 地域:東日本
  * 可用性オプション:no
  * イメージ:Ubuntu Server 18.04 LTS
  * Azureスポットインスタンス:no
  * サイズ:Standard_B1ls

* 管理者アカウント:
  * 認証の種類:SSH公開キー
  * ユーザ名:azureuser
  * SSHの公開キーソース:新しいキーの組みを構成
  * キーの組名:ssh-test-ubuntu1804_key

* 受信ポートの規則:
  * パブリック受信ポート:選択したポート
  * 受信ポートを選択:ssh(20)

* 作成後に秘密のキーのダウンロードとリソースの作成

### SSH (Mac)から接続する
1. ダウンロードした秘密キーを ~/.ssh に移動する
2. 秘密キーの権限を変更する
~~~
> chmod 600 <秘密キーファイル名>
~~~
3. ssh で接続する
~~~
> ssh -i <秘密キーファイル名> azureuser@<サーバ名>
~~~

### scp でファイルをアップロード
~~~
> scp -i  <秘密キーファイル名> <ローカルファイル名> azureuser@<サーバ名>:~/
~~~

## Azure に VM を作成して SSH で接続する(windows)

### Azure に VM を作成する
リソースの作成 > Compute > 仮想マシン

### SSH を有効にする
1. 実行コマンド > RunPowerShellScript
2. スクリプトを実行する

https://www.server-world.info/query?os=Windows_Server_2019&p=ssh&f=1

~~~
> Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
> Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
> Start-Service -Name "sshd"
> Set-Service -Name "sshd" -StartupType Automatic
> Get-Service -Name "sshd" | Select-Object *
> New-NetFirewallRule -Name "SSH" `
-DisplayName "SSH" `
-Description "Allow SSH" `
-Profile Any `
-Direction Inbound `
-Action Allow `
-Protocol TCP `
-Program Any `
-LocalAddress Any `
-RemoteAddress Any `
-LocalPort 22 `
-RemotePort Any
~~~

3. ssh 接続する
~~~
ssh azureuser@<サーバ名>
~~~

4. PowerShell を実行する
~~~
powershell
~~~

5. dotnet をインストールするスクリプトをコピーする

https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-install-script

~~~
scp ./dotnet-install.ps1 azureuser@<サーバ名>:c:\users\azureuser\dotnet-install.ps1
~~~

6. dotnet をインストールする
~~~
./dotnet-install.ps1 -Channel 3.1 -InstallDir C:\cli
~~~


