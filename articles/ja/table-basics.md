# テーブルの基本

### 基本仕様

`NonNonTable` は `Serializable` なクラスを List として持つ `ScriptableObject` です。\
Unity エディタ上で表形式にレンダリングされ、各フィールドをセル単位で編集可能にします。

<img src="~/images/image (6).png" alt="">

セルはフィールドの型に応じて形を変えます。

例えば、int 型のフィールドに対しては IntegerField を、bool 型のフィールドに対しては Toggle  を持ったセルが生成されます。

専用セルが用意されている型は以下の通りです。

<details>

<summary>専用セルが用意されている型</summary>

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
* `[Flags] enum`
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

<img src="~/images/image (16).png" alt="">

専用セルが用意されていない型の場合、値を `ToString()` した文字列がセルに表示されます。

また、セルをダブルクリックすると PropertyField を持つ Popup が表示され、値を編集することができます。

<img src="~/images/image (17).png" alt="">

配列の場合、各要素を , 区切りにした文字列がセルに表示されます。

また、配列も同様にダブルクリックにより Popup が表示され、内容を編集することができます。

### &#x20;制約

`NonNonTable` は実装する上でいくつかの制約があります。

* `partial` なクラスである
* データ型 `<T>` は `class` のみ、`struct` はサポート対象外
