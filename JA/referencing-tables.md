---
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# 他のテーブルを参照する

{% columns %}
{% column width="50%" %}
<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column width="50%" %}
他のテーブルのデータをドロップダウンで選択できるようにすることも可能です。

保存されるのはデータそのものではなく、参照されるテーブルのデータクラスの**キー**になります。

他のテーブルを参照するために必要な作業は主に以下の4つです。

* 参照されるテーブルのデータクラスへの `IRelationalData<T>` の実装
* 参照指定のための Attribute の実装
* カスタムセルの実装
* 参照するテーブルのフィールドへ Attribute を付与

int 値をキーとするテーブルを例に実装の流れを追ってみましょう。
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
[NonNonTable]
public partial class SampleTable : NonNonTable<SampleData> { }

[Serializable]
public class SampleData : IRelationalData<int> 
{ 
    public int Key;
    public string Name;

    int IRelationalData<int>.Id => Key;
    string IRelationalData<int>.DisplayName => $"{Key}: {Name}";
}
```
{% endcolumn %}

{% column %}
### データクラスへの IRelationalData の実装

あるテーブルを他のテーブルから参照するためには、テーブルのデータクラスに `IRelationalData<T>` を実装する必要があります。

`T` で指定した型がそのデータクラスのキーとなります。

また、DisplayName で生成した文字列がセルに表示される文字列となります。

{% hint style="info" %}
参照されるテーブルは ProjectSettings/NonNonSheet の Tables に登録されている必要があります。テーブルアセットは作成時点で自動登録されますが、セルに「Table not found」と表示された場合はテーブルアセットが登録されているかを確認してください。\
また、同じデータクラスを使用したテーブルが複数登録されている場合、登録順の早いものが参照されるため注意してください。
{% endhint %}
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class SampleTableRefAttribute : CellCustomAttribute { }
```
{% endcolumn %}

{% column %}
### 参照指定のための Attribute の実装

`CellCustomAttribute` を継承した Attribute を実装します。命名に特に制約はありません。

ここで定義した Attribute がテーブル参照用フィールドを示す属性として使用されます。
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
#if UNITY_EDITOR
[NonNonCell]
public class SampleTableRelationCell : DataRelationCell<int, SampleTableRefAttribute, SampleData> { }
#endif
```
{% endcolumn %}

{% column %}
### カスタムセルの実装

`DataRelationCell` を継承したクラスを実装します。

`DataRelationCell` の型引数は

* 第１型引数: `IRelationalData<T>` の T と同じ型
* 第２型引数: `CellCustomAttribute` を継承した Attribute クラス
* 第３型引数: `IRelationalData<T>` を実装したデータクラス

また、このクラスは Editor only なアセンブリに配置するか、`#if UNITY_EDITOR` で囲む必要があるため注意してください。
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class MultiSampleTableRefAttribute : CellCustomAttribute { }

#if UNITY_EDITOR
[NonNonCell]
public class SampleTableMultiRelationCell : MultiDataRelationCell<int, MultiSampleTableRefAttribute, SampleData> { }
#endif
```
{% endcolumn %}

{% column %}
`DataRelationCell` の代わりに `MultiDataRelationCell` を継承したクラスを実装することで、複数のキーを保存させることもできます。

<div align="left"><figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure></div>
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
[NonNonTable]
public partial class ReferencingTable : NonNonTable<ReferencingData> { }

[Serializable]
public class ReferencingData
{
    [SampleTableRef] public int SampleTableKey;
    [MultiSampleTableRef] public int[] SampleTableKeys;
}
```
{% endcolumn %}

{% column %}
### 参照するテーブルのフィールドへ Attribute を付与

参照されるテーブルとは別のテーブルを実装します。

データクラスに `IRelationalData<T>` の T と同じ型のフィールドを実装しましょう。

そのフィールドに  `CellCustomAttribute` を継承した Attribute（今回だと `SampleTableRefAttribute` または `MultiSampleTableRefAttribute`）を付与します。
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
以上で ReferencingTable から SampleTable を参照することができるようになりました。

今回の例では int 値をキーにしましたが、 string や enum をキーとすることも可能です。


{% endcolumn %}
{% endcolumns %}

