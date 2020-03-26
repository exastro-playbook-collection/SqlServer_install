# Ansible Role: SqlServer\_install
===============================

## Description

本Ansible RoleはMicrosoftのリレーショナルデータベース管理ソフトウェアSQLServerのインストールおよびインスタンス作成を行います。<br/>
対象バージョンは以下のバージョンです。
~~~
	SQLServer 2016 SP1はExpress edition版やDeveloper edition版などがあるので、  
	インストールしたいもののみダウンロードしてください。  

	Microsoft Data platfrom  
	https://www.microsoft.com/en-us/sql-server/sql-server-2016  
~~~
設定内容は以下のとおりです。

- 事前のチェック処理
	- OSバージョン
	- SQLServerインスタンスが既に存在するか
- インストールの実施
	- INIファイルを生成する
	- インストール

## Supports

- 管理マシン(Ansibleサーバ)
 * Linux系OS（RHEL7）
 * Ansible バージョン 2.4 以上 (動作確認バージョン 2.4, 2.9)
 * Python バージョン 2.x, 3.x  (動作確認バージョン 2.6, 2.7, 3.6)

- 管理対象マシン(構築対象マシン)
 * Windows Server 2016
 * Framework バージョン 3.0 以上
 * Management Framework バージョン 3.0 以上

## Requirements

- 管理マシン(Ansibleサーバ)
  * 管理対象マシンと管理者権限でPowershell通信可能であること。
- 管理対象マシン(構築対象マシン)
  * 管理対象マシンにアクセスできるように配置

## Role Variables

Role の変数値について説明します。

### Mandatory variables

以下の変数は、実行時に、必ず指定します。  

- SQLServer共通変数設定
	* VAR\_SQLServer\_OS\_Version：ターゲットホストのOSバージョン  
		* 例：Windows Server 2016
	* VAR\_Tmp\_Folder：インストール用一時フォルダ  
		* 例：C:\Temp\AnsibleforSQLServer  
	* VAR\_Installer\_Type：インストール物件の類型を指定する  
		* 例：ISO
	* VAR\_Installer\_Unzip\_Path：展開されたインストール物件のフルパスを指定する  
		* 例：E:\		 
	* VAR\_AllowSkip\_SQLServerSWExist:  
        既にSQLServerインスタンスがインストールされた場合、
        処理を中止しないで、事前準備ロールとSQLServerインストールロールをスキップする/しない：  
        true/false（デフォルトはfalse）  
		* 例：true
	* VAR\_INI\_Path：SQLServerインストール用INIファイルの格納パス  
		* 例：C:\Temp\AnsibleforSQLServer  
	* VAR\_Installer\_Name：SQLServerインストールパッケージのファイル名（ISOファイル）
		* 例：SQLServer2016SP1-FullSlipstream-x64-JPN.iso
</br>
- SQLServerインストール Role変数
	* VAR\_Extra\_Command：INIファイルに指定項目以外のオプションを設定しない場合、ここに指定してsetup.exeのコマンドラインに追加される。  
		* 例：/SQLSVCACCOUNT="NT AUTHORITY\SYSTEM"  
- INI用変数  
	* VAR\_INSTANCENAME: INIファイルINSTANCENAME変数  
		* 例：MSSQLSERVER  
	* VAR\_ACTION: INIファイルACTION変数  
		* 例：Install  
	* VAR\_FEATURES: INIファイルFEATURES変数  
		* 例：SQLENGINE  
	* VAR\_SQLSYSADMINACCOUNTS: INIファイルSQLSYSADMINACCOUNTS変数  
		* 例：Administrator

### Optional variables

以下の変数は特定状況で指定します。  
※下記の変数の意味と使用方法については、インストール時に設定可能なパラメータファイルの要件を参照してください。

- INI用変数
  - VAR\_INSTANCEDIR:                   
  - VAR\_SUPPRESSPRIVACYSTATEMENTNOTICE:   
  - VAR\_ENU:                        
  - VAR\_UpdateEnabled:              
  - VAR\_USEMICROSOFTUPDATE:         
  - VAR\_UpdateSource:               
  - VAR\_HELP:                       
  - VAR\_INDICATEPROGRESS:           
  - VAR\_X86:                        
  - VAR\_INSTALLSHAREDDIR:           
  - VAR\_INSTALLSHAREDWOWDIR:        
  - VAR\_SQLTELSVCACCT:              
  - VAR\_SQLTELSVCSTARTUPTYPE:       
  - VAR\_AGTSVCACCOUNT:
  - VAR\_AGTSVCPASSWORD            
  - VAR\_AGTSVCSTARTUPTYPE:          
  - VAR\_COMMFABRICPORT:             
  - VAR\_COMMFABRICNETWORKLEVEL:     
  - VAR\_COMMFABRICENCRYPTION:       
  - VAR\_MATRIXCMBRICKCOMMPORT:      
  - VAR\_SQLSVCSTARTUPTYPE:          
  - VAR\_FILESTREAMLEVEL:            
  - VAR\_ENABLERANU:                 
  - VAR\_SQLCOLLATION:               
  - VAR\_SQLSVCACCOUNT:
  - VAR\_SQLSVCPASSWORD:         
  - VAR\_SQLSVCINSTANTFILEINIT:      
  - VAR\_SQLTEMPDBFILECOUNT:       
  - VAR\_SQLTEMPDBFILESIZE:        
  - VAR\_SQLTEMPDBFILEGROWTH:      
  - VAR\_SQLTEMPDBLOGFILESIZE:     
  - VAR\_SQLTEMPDBLOGFILEGROWTH:   
  - VAR\_ADDCURRENTUSERASSQLADMIN:   
  - VAR\_TCPENABLED:               
  - VAR\_NPENABLED:                
  - VAR\_BROWSERSVCSTARTUPTYPE:    
  - VAR\_SAPWD :                   
  - VAR\_SECURITYMODE:             
  - VAR\_PID:                      
  - VAR\_ASSYSADMINACCOUNTS:  
  - VAR\_ASSVCACCOUNT:  
  - VAR\_ASSVCPASSWORD:  
  - VAR\_ISSVCACCOUNT:  
  - VAR\_ISSVCPASSWORD:  
  - VAR\_RSSVCACCOUNT:  
  - VAR\_RSSVCPASSWORD:

### Unchanging variables  
  * OS\_Language\_Code:  インストール実施時、管理対象マシン側のOS言語を指定する。日本語の場合MS932とする。

## Dependencies  

特にありません。  

## 制限事項  


## 注意事項  

* 変数関連  
 * 必須変数は必ず指定してください。  
 * INIファイルの変数にtrue / falseがある場合は、 ""（二重引用符）を使用してブール変数としてエスケープされないようにします。
 * 変数VAR\_SQLServer\_OS\_Versionの書き方は：Windows Server 2016 大小文字区別、半角スペースあり。OSバージョンを検証する時、完全一致のチェックではなく、部分一致となります。つまり、実際のOSはWindows Server 2016、変数VAR\_SQLServer\_OS\_VersionにWindows Serverが設定されても一致する結論となります。ただ、Win Serverのような値は一致しないと判断されます。  
 * SqlServerのサイドルールに準拠する必要があるたとえば、インスタンス名は16文字に制限されています。この間違いインストール時のみ反映されます，Ansibleチェックしません。  
 * VAR\_Installer\_TypeがISOあるいはCMPの場合、Role：SQLServerインストールのみ実施する時、AnsibleでISOファイルあるいはCMPファイルを展開していないため、手動的にそのISOファイルあるいはCMPファイルを展開する必要。そのとき、展開されたフルパスを事前に設定する必要（例：E:\）。VAR\_Installer\_TypeがPATHの場合、直接に展開されたインストール物件のフルパスを設定してください。  
* 事前準備  
 * AllVariable.yml変数ファイルを使用せず、Role自身の変数を使用する場合は、共通変数の設定が同じであることを確認してください。  

## Usage  

1. 本Roleを用いたPlaybookを作成します。  
2. SQLServerインスールバイナリは下記のMirosoftサイトからダウンロードして、Ansible管理マシンに配置します。SQLServer 2016 SP1はExpress edition版やDeveloper edition版などがあるので、インストールしたいもののみダウンロードしてください。  
	Microsoft Data platfrom  
	https://www.microsoft.com/en-us/sql-server/sql-server-2016  
3. 必須変数は必ず指定してください。  
4. 任意変数を必要に応じて指定します。  
5. Playbookを実行します。  

## Example Playbook

今回既にPlaybookの形式を提供したが、もし自分でPlaybookを作成する時、以下の記述を参照してください。  
インストールとすべての設定をする場合は、提供したRoleを"roles"ディレクトリに配置したうえで、
以下のようなPlaybookを作成できます。

- フォルダ構成
~~~
  - group_vars/
      * server1
      * server2
  - host_vars/
      * host1
      * host2
  - roles/
      * SqlServer_install/
      * SqlServer_preinstall/
  - sqlserverinstall.yml
  - AllVariable.yml
  - hosts
~~~

- マスターPlaybook サンプル「sqlserverinstall.yml」
~~~
# sqlserverinstall.yml
---
  - hosts: sql
    roles:
      - role: SqlServer_preinstall
        tags:
          - SqlServer_preinstall   
      - role: SqlServer_install
        tags:
          - SqlServer_install
~~~

## Running Playbook

- extra-varsを利用する場合の実行例

~~~sh
> ansible-playbook sqlserverinstall.yml --extra-vars="@./AllVariable.yml" -i ./hosts  
~~~
- 本roleのみを実行する場合は

~~~sh
> ansible-playbook sqlserverinstall.yml --extra-vars="@./AllVariable.yml" --tags "SqlServer\_install" -i ./hosts  
~~~


# Copyright
---------
Copyright (c) 2017 NEC Corporation

# Author Information
------------------
NEC Corporation
