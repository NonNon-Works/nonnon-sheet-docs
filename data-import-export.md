# csv・jsonをインポート・エクスポートする

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
NonNonTable は csv・ json 形式でのインポート・エクスポートをサポートしています。

NonNonTable アセットのインスペクタから実行することができます。
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
