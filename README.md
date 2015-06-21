# ansible-elasticsearch+kibana

## What's this
ansibleを使って、ログデータの収集と全文検索可能なマシンを構築します。  
以下のソフトウェアをインストールします。  
elasticsearch…ログデータのリアルタイム全文検索・分析エンジン  
kibana3…ログの可視化ソフトウェア  
fluentd…ログデータの収集ソフトウェア  

ansible...サーバ構成管理ソフトウェア  

## Execution Environment

```
$ ansible --version  
ansible 1.6.2 

$ gem list | grep serverspec  
serverspec (0.14.0)
```

## Install Item
+ elasticsearch
 - plugin bigdesk  
 - plugin head  
+ kibana3
+ fluentd
+ nginx

## Usage
1. ansibleを実行するマシン上で、当プロジェクトをclone  

1. SSH公開鍵認証の準備  

    * プロビジョニング対象サーバにSSH公開鍵認証方式でログイン出来るように準備してください。

1. 設定ファイル変更  

    * ./hosts
        * ansibleのhost設定ファイル
        * プロビジョニング対象サーバのIPアドレス(host名)を変更してください。
    * ./site.yml内の${hostname}と${hostip}を変更
        * ansible内で使用する変数

1. ansible playbook 実行  

        $ ansible-playbook site.yml -i hosts  

    たまにyumで失敗することがありますが再度実行するとうまくいくことがあります。
    --checkオプションをつけるとでdry-run実行になります。

1. テストの実行  

        $ rake serverspec:Install-Elasticsearch-Kibana

1. kibana3へのアクセス  

        http://IPアドレス/  

    * 画面上部に次のエラーがでますが、elasticsearch上にデータがないとでるようです。  

        Error Could not find http://192.168.0.109:9200/_all/_mapping. If you are using a proxy, ensure it is configured correctly

1. elasticsearchのプラグインbigdeskへのアクセス  
次のURLでアクセスできます。  

        http://IPアドレス:9200/_plugin/bigdesk  

1. elasticsearchのプラグインheadへのアクセス  
次のURLでアクセスできます。  

        http://IPアドレス:9200/_plugin/head  

## Caution
* fluntdの収集先  
    * 初期設定では、fluentdのログ収集先は/var/log/nginx/access.logにしています。  
    * つまり、構築後にkibanaに何度かアクセスすると、elascticsearchにログが保存されます。  
    * 動作確認用なので、早めに設定を変えてください。  

