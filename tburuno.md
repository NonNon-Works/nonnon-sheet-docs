# テーブルの基本

### 基本仕様

`NonNonTable` は `Serializable` なクラスを List として保持する `ScriptableObject` です。\
Untiy エディタ上で表形式にレンダリングされ、各フィールドをセル単位で編集可能にします。

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
セルはフィールドの型に応じて形を変えます。

例えば、int 型のフィールドに対しては IntegerField を持ったセルが、bool 型のフィールドに対しては Toggle  を持ったセルが生成されます。

専用のセルをサポートする型は以下の通りです。
{% endcolumn %}
{% endcolumns %}

<details>

<summary>サポートする型</summary>

* `bool`
* `sbyte`
* `byte`
* `short`
* `ushort`&#x20;
* `int`&#x20;
* `uint`&#x20;
* `long`&#x20;
* `ulong`&#x20;
* `float`
* `double`
* `string`
* `enum`
* `[Falges] enum`
* `Vector2`
* `Vector3`
* `Vector4`
* `Vector2Int`
* `Vector3Int`
* `Rect`
* `Bounds`
* `Color`
* `LayerMask`&#x20;
* `UnityEngine.Object`（`GameObject`, `MonoBehaviour`, `ScriptableObject`, `Texture` など）
* `AssetReference`（Addressables が有効なときのみ）

</details>

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
専用のセルがサポートされていない型の場合、セルには値を `ToString()` した文字列が表示されます。

また、セルをダブルクリックすると PropertyField を持った Popup が表示され、値を編集することができます。


{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
配列の場合、セルには各要素を , 区切りにした文字列が表示されます。

また、配列も同様にダブルクリックにより Popup が表示され、内容を編集することができます。
{% endcolumn %}
{% endcolumns %}

### &#x20;制約

`NonNonTable<T>` は実装する上でいくつかの制約があります。これら

* `partial` なクラスである
* データ型 `<T>` は `class` のみ、`struct` はサポート対象外
