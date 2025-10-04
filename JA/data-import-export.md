# csv・jsonをインポート・エクスポートする

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
NonNonTable は csv・ json 形式でのインポート・エクスポートをサポートしています。

NonNonTable アセットのインスペクタから実行することができます。

この機能は Unity エディタでのみ動作する機能です。ビルドでは使用できないので注意してください。
{% endcolumn %}
{% endcolumns %}

## csv

以下の形式の csv のインポート・エクスポートに対応しています

* フィールドセパレータ：`,`&#x20;
* ヘッダ行：有
  * インポート時はヘッダ行をスキップするのみなので、必ずしもヘッダー行の内容が正しいものである必要はありません

## json

以下の形式の json のインポート・エクスポートに対応しています

* トップレベルが配列
* 配列要素はテーブルのデータ型
