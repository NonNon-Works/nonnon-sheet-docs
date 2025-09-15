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
{% column %}
<div align="left"><figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure></div>
{% endcolumn %}

{% column %}
あるテーブルのデータを他のテーブルからドロップダウンで選択することもできます。

保存されるのはデータそのものではなく、参照されるテーブルのデータクラスの**キー**なので注意してください。
{% endcolumn %}
{% endcolumns %}

int値をキーとするテーブルを例にテーブルの参照関係を作ってみましょう。

まずは参照されるテーブル側の実装についてです。

```csharp
[NonNonTable]
public partial class IntKeyReferencedTable : NonNonTable<IntKeyData> { }

[Serializable]
public class IntKeyData : IRelationalData<int>
{
    public int Id;
    public string Name;

    int IRelationalData<int>.Id => Id;
    string IRelationalData<int>.DisplayName => $"{Id}: {Name}";
}

public class IntKeyDataRelationAttribute : CellCustomAttribute { }

#if UNITY_EDITOR
[NonNonSheet.Editor.NonNonCell]
public class IntKeyDataRelationCell : NonNonSheet.Editor.DataRelationCell<int, IntKeyDataRelationAttribute, IntKeyData> { }
#endif
```



{% columns %}
{% column %}
```csharp
[Serializable]
public class IntKeyData : IRelationalData<int>
{
    public int Id;
    public string Name;

    int IRelationalData<int>.Id => Id;
    string IRelationalData<int>.DisplayName => $"{Id}: {Name}";
}
```
{% endcolumn %}

{% column %}
IntKeyData クラスが `IRelationalData<int>` を実装してる点に注目してください。\
こうすることで IntKeyData をデータクラスとして用いるテーブルは他のテーブルから参照されることができます。

IRelationalData は T Id と string DisplayName の実装が必要です。

T Id は参照先で実際に保存される値となります。

string DisplayName は後述する IntKeyDataRelationCell のセル内で表示される文字列になります。
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class IntKeyDataRelationAttribute : CellCustomAttribute { }
```
{% endcolumn %}

{% column %}
IntKeyDataRelationAttribute は参照する側のテーブルのデータクラスにおいて、キーを保存するフィールドに付与するための CellCustomAttribute です。
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
#if UNITY_EDITOR
[NonNonSheet.Editor.NonNonCell]
public class IntKeyDataRelationCell : NonNonSheet.Editor.DataRelationCell<int, IntKeyDataRelationAttribute, IntKeyData> { }
#endif
```
{% endcolumn %}

{% column %}
IntKeyDataRelationCell は参照を設定するようのカスタムセルの定義です。

DataRelationCell のサブクラスとして実装します。

DataRelationCell の第一型引数にはキーの型を、第二型引数にはリレーション用属性を、第三型引数には参照される側のデータクラスを指定します。
{% endcolumn %}
{% endcolumns %}

また、参照されるテーブルは必ずDatabaseに登録しておく必要があります。

{% hint style="info" %}
同じIRelationalDataなデータクラスを使用したテーブルが複数データベースに登録されている場合、登録順の早いものが参照されます。\
例えばIntKeyDataをデータクラスとするテーブルが複数あったり、IntKeyReferencedTable の ScriptableObject が複数作成されていたりすると上記のような動作が起こります。しかし、このような使用法は推奨しません。なるべく避けるようにしてください。
{% endhint %}

次に参照するテーブル側の実装についてです。

```csharp
[NonNonTable]
public partial class ReferencingTable : NonNonTable<ReferencingData> { }

[Serializable]
public class ReferencingData
{
    [IntKeyDataRelation] public int IntKeyRelation;
}
```

参照したいテーブルが提供するキーの型と同じ型のフィールドにリレーション用属性を付与しましょう。

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
これで ReferencingTable から IntKeyReferencedTable を参照できるようになりました
{% endcolumn %}
{% endcolumns %}
