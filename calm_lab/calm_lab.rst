.. _calm_lab:

-----------------
Calm演習
-----------------

ブループリント解読
++++++++++++++++++++++++++

#. アプリケーションプロファイルの”作成”メニューを選択します。アプリケーションを構成するサービス間の依存性が確認出来ます。このアプリケーションの起動時の動作は以下となっています。

   .. figure:: images/lab1.png

[IPAM_CLIENTサービス]
  - Pre-Create - 何もしない
  - Substrate Create - 既存の仮想マシン(10.42.10.56)に対してSSH接続
  - Package Install - 何もしない
  - Service Create - assign_ipaddress.shのシェルスクリプトを使用してIPアドレスとサブネットマスクを割り当て
  - Service Start - 何もしない

[CENTOS_SERVICEサービス]
  - Pre-Create - 何もしない
  - Substrate Create - Nutanix AHV上に新規仮想マシン(vCPU:2, Core/vCPU:1, Memory:4GiB, Image: CentOS7)で作成、NICはPrimary(eth0)とCalm-Lab(eth1)の2つをアタッチし、eth1にIPAM_CLIENTから割当てられたIPアドレスとサブネットマスクを設定。
  - Package Install - Docker、Calm DSLをインストール、(バグ混入)
  - Service Create - 何もしない
  - Service Start - Dockerサービスのenable確認、calm-dsl dockerコンテナの動作確認、(バグ混入確認)

アプリケーション起動のトラブルシューティング(マクロ編)
++++++++++++++++++++++++++

#. ブループリント編集画面から"起動"をクリックし、アプリケーションの名前に"あなたのイニシャル-CENTOS-IPAM-CALMDSL"、ATTENDEE_IDにあなたの参加者IDを入力し、"作成"をクリックします。

   .. figure:: images/lab2.png

#. ”アプリケーション”画面に遷移し、”監査”タブに移動するとアプリケーションの起動プロセスが確認できます。ここではスクリプトへの引数が誤っているように見受けられます。スクリプト仕様としてあなたのATTENDEE_IDを引数として入力しなければいけないようです。

   .. figure:: images/lab3.png

#. ”ブループリント”作成画面に戻り、"IPAM_CLIENT_SERVICE"の作成アクションに"Assign IP address"のタスクがあり、ここでassign_ipaddress.shのシェルスクリプトを実行しているようです。

   .. figure:: images/lab4.png

#. ”Assign IP address”タスクを修正していきます。アプリケーション起動時に指定する”ATTENDEE_ID”という変数を引数として与えるため、Calmのマクロを用いて@@{ATTENDEE_ID}@@を与えます。”保存”をクリックします。

   .. figure:: images/lab5.png

#. ちなみに、ATTENDEE_IDの定義は”アプリケーションプロファイル”-->"Default"に移動すると確認出来ます。これを定義しておくことで、アプリケーション起動時にパラメータ（@@{hoge}@@の書式、マクロといいます)として使用可能となります。

   .. figure:: images/lab6.png

アプリケーション起動のトラブルシューティング(バグ編)
++++++++++++++++++++++++++

#. ブループリント編集画面から"起動"をクリックし、アプリケーションの名前に"あなたのイニシャル-CENTOS-IPAM-CALMDSL2"、ATTENDEE_IDにあなたの参加者IDを入力し、"作成"をクリックします。

#. ”アプリケーション”画面に遷移し、”監査”タブに移動するとアプリケーションの起動プロセスが確認できます。ここではCENTOS_SERVICEの"Service Start"アクションでバグを発見したようです。

   .. figure:: images/lab7.png

#. 確認すると、CENTOS_SERVICEの”Package Install”の”Create tmp file”ステップでバグが混入し、"Service Start"アクションでバグ検知をしているようです。

   .. figure:: images/lab8.png
   .. figure:: images/lab9.png

#. ”Create tmp file”タスクを修正していきます。ここでは”Create tmp file”タスクを削除します。タスクの右側にあるゴミ箱をクリックして下さい。”保存”をクリックします。

   .. figure:: images/lab9.png

#. ブループリント編集画面から"起動"をクリックし、アプリケーションの名前に"あなたのイニシャル-CENTOS-IPAM-CALMDSL3"、ATTENDEE_IDにあなたの参加者IDを入力し、"作成"をクリックします。今度はうまくいくはずです。

Calm DSL
++++++++++++++++++++++++++

#. Calmの”アプリケーション”から”サービス”タブを開き、”CENTOS_SERVICE”サービスの”ターミナルを開く”をクリックします。認証情報は"CENTOS_CRED"を使用して下さい。

   .. figure:: images/lab9.png

#. Dockerコンテナを起動、対話側シェルに入り、Calm DSLのPythonによるブループリントの作成までを行います。

   .. note::

      - docker run -it ntnx/calm-dsl -> Calm DSLのコンテナイメージからコンテナを起動し、対話型シェルを表示
      - calm init dsl : Calm DSLの初期設定
      - Prism Central IP []: X.X.X.X -> あなたのPrism Central IPアドレス
      - Port [9440]:  -> デフォルト値を仕様のためEnter
      - Username [admin]: -> デフォルト値を仕様のためEnter
      - Password []: techX2020! -> PCのパスワード
      - Project [default]: BootcampInfra -> プロジェクト名 
      - calm init bp -> サンプルブループリントをローカル作成

#. サンプルブループリントのHelloBlueprintは以下のようなディレクリ構成になっています。それぞれのファイルの中身を確認してみて下さい。

   .. figure:: images/lab10.png            

#. サンプルブループリントをCalmに読み込ませ、アプリケーション起動を行います。

   .. note::

      - calm create bp --file HelloBlueprint/blueprint.py --name “あなたのイニシャル”-HelloDSL -> サンプルブループリントをCalm上で作成
      - calm get bps -> Calm上のブループリントをリスト
      - calm describe bp “あなたのイニシャル”-HelloDSL -> Calm上のブループリントの詳細表示
      - calm launch bp “あなたのイニシャル”-HelloDSL --app_name “あなたのイニシャル”-HelloDSL –i -> ブループリントからアプリケーションを起動
      - calm get apps -> Calm上のアプリケーションをリスト
      - calm describe app “あなたのイニシャル”-HelloDSL” -> Calm上のアプリケーションの詳細表示

#. Calm UIに入り、Calm DSLから行ったブループリントのアップロードとアプリケーション起動が反映されていることを確認します。

   .. figure:: images/lab11.png            
   .. figure:: images/lab12.png            


