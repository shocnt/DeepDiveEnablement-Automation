.. _prismpro_prep:

-----------------
Prism Pro演習準備
-----------------

テスト用VMのプロビジョニング
++++++++++++++++++++++++++

#. adminユーザでPrism Centralにログイン、**仮想マシンを作成**をクリックし、次のフィールドに入力して仮想マシンを作成して下さい。

   - **Select a cluster to create the VM on.** - PHX-POCXXX
   - **Name** - ”あなたのイニシャル”-LinuxToolsVM
   - **Select Disk Images** - Linux_ToolsVM.qcow2
   - **vCPU(s)** - 2
   - **Number Of Cores Per vCPU** - 1
   - **Memory** - 2 GiB
   - **Disk** - "+ Add New Disk"-->”イメージサービスからクローン”-->"Linux-LinuxToolsVM.qcow2"
   - **Network** - ”Primary”ネットワークのみ接続済みでアタッチ

#. **Save**をクリックし、作成される仮想マシンにチェックボックスが入った状態で**パワーオン**を実施。

GTSPrismOpsLabUtilityServerの準備
++++++++++++++++++++++++++

#. **Prism Central** にて **仮想マシン（VMs）** ページに移動し、 **GTSPrismOpsLabUtilityServer** のIPアドレスを控えます。

   .. figure:: images/init1.png

#. ブラウザを起動し、http://`<GTSPrismOpsLabUtilityServer_IP_ADDRESS>`/alerts に接続します。[例 http://10.42.113.52/alerts] 下図のようなログイン画面が表示された場合は **Prism Central IP** と、Prismログインのための **Username**  **Password** を入力し **Login** をクリックします。

   .. figure:: images/init2.png

#. 下図のようなページが表示されたら、タブを開いたままにします。このラボの後の部分で使用します。

   .. figure:: images/init2b.png

#. また別のタブで、 http://`<GTSPrismOpsLabUtilityServer_IP_ADDRESS>`/ にアクセスします。 [例 http://10.42.113.52/] このURLのPrismUIを利用して演習を進めます。

   .. figure:: images/init3.png

