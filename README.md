# proxy_batcher
複数のアプリケーションのプロキシの設定コマンドを一度に叩く

## 今考えてる仕様
特定のファイルに以下の書式で設定を書いて、コマンドで呼べるようにする。

|書式|例|やること|
|:--|:-|:-----|
|`HTTP:`|`HTTP: http://proxy:port/`|HTTPプロキシを設定する|
|`HTTPS:`|`HTTPS: http://proxy:port/`|HTTPSプロキシを設定する|
|`[?***]`|`[?git]`|確認セクションの開始|
|`[!***]`|`[!git]`|実行セクションの開始|
|`<http>`|`<http>`|これが設定されたHTTPプロキシのアドレスになる|
|`<http>=`|`<http>=...`|この右に、現在のHTTPプロキシを出力するコマンドを書く|
|`<https>`|`<https>=`|これが設定されたHTTPSプロキシのアドレスになる|
|`<https>=`|`<https>=`|この右に、現在のHTTPSプロキシを出力するコマンドを書く|

コマンドのグループは、「セクション」ごとに分ける(git系のコマンドならgitセクションといった感じ)<br>
確認セクションと実行セクションの名前は一致している必要がある

```
# 設定するプロキシのアドレスを書く
HTTP: http://some.goodness.proxy.id:port/
HTTPS: https://some.goodness.proxy.id:port/

# 失敗したとき用にもとに戻せるように、現在のプロキシ設定を確認するための
# コマンドを書き残しておく
# なくても問題ないが、エラーが出た時に戻せない
#
[?git]
<https>=git config --global https.proxy
<http>=git config --global http.proxy

[?docker]
<https>=echo $https_proxy
<http>=echo $http_proxy

# 実際に設定を行う
[!git]
git config --global http.proxy <http>
git config --global https.proxy <https>

[!docker]
export http_proxy=<http>
export https_proxy=<https>
```
<br>
<br>
<br>
<br>
<br>

`TODO: READMEをどうにかする`
