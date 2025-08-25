-----

# 📖 NaaSユーザ向け操作手順書

## ⚠️ 基本事項と注意事項

  - **NaaSユーザの対応は、必ずパーティションを変更して操作してください。**
  - 通常運用における設定変更は、すべてパーティション内で実施します。
  - パーティション内の設定内容は、パーティションなしのMLBの設定手順に準拠します。
  - `shared`パーティションはベースとなるパーティションで、ログイン時の初期パーティションです。
  - お客様のパーティション名は、`nssv2xxxx`の形式となります。
  - 設定を行う際は、**対象のパーティションが正しいことを必ず確認してください。**

-----

## 💻 CLI操作手順

### パーティションへのログイン

1.  初期ログインは`shared`パーティションになります。

    ```
    fk1-naas-lb01a#
    ```

2.  以下のコマンドで、作業対象のパーティションへ移動します。

    ```
    fk1-naas-lb01a# active-partition <パーティション名>
    ```

    💡 ヒント：利用可能なパーティションの一覧は、`active-partition ?`コマンドで確認できます。

3.  プロンプトが移動先のパーティション名に変わったことを確認します。

    ```
    fk1-naas-lb01a-Active[nssv99999]#
    ```

    **【例】**

    ```
    fk1-naas-lb01a# active-partition nssv99999
    Current active partition: nssv99999
    fk1-naas-lb01a-Active[nssv99999]#
    ```

### パーティションの移動

  - パーティション間を直接移動できます。一度`shared`パーティションに戻る必要はありません。
    ```
    fk1-naas-lb01a-Active[nssv99999]# active-partition <移動先パーティション名>
    ```
    **【例】**
    ```
    fk1-naas-lb01a-Active[nssv99999]# active-partition nssv20088
    # nssv20088へ移動
    Current active partition: nssv20088
    fk1-naas-lb01a-Active[nssv20088]#
    ```

### ログアウト

  - パーティション内から直接ログアウトできます。
    ```
    fk1-naas-lb01a-Active[nssv99999]# exit
    Are you sure to quit (N/Y)?: y
    Connection to 172.22.53.215 closed.
    ```
    **【例】**
    ```
    fk1-naas-lb01a-Active[nssv20088]# exit
    Are you sure to quit (N/Y)?: y
    Connection to 172.22.53.215 closed.
    [a21039162@icfs1-steplin1 ~]$
    ```

-----

## 🌐 GUI操作手順

### パーティションへのログイン

1.  GUIの画面で をクリックします。
2.  表示されるパーティションリストから、対象のパーティション名をクリックします。
    💡 ヒント：パーティション名が省略されて表示されるため、カーソルを合わせるとフルネームを確認できます。
3.  をクリックして選択を確定します。
![Githubのロゴ](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)

### 💾 設定の保存と同期

#### 設定の保存（パーティション内で実施）

  - 設定変更後は、以下のコマンドで保存してください。
    ```
    fk1-naas-lb01a-Active[nssv20088]# wr mem
    ```
  - `primary`/`secondary`は不要です。誤って入力するとエラーになります。

#### 設定の同期（`shared`パーティションで実施）

  - **設定の同期に不明点がある場合は、運用管理（OC）に確認してください。**
  - `shared`パーティションにログインした状態で、以下のコマンドを実行します。
    ```
    fk1-naas-lb01a# configure
    fs1-naas-lb01a(config)# configure sync all partition <パーティション名> auto-authentication <Standby IP>
    ```
