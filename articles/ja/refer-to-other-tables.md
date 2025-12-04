# 他のテーブルを参照する

<img src="~/images/image (18).png" alt="">

他のテーブルのデータをドロップダウンで選択できるようにすることも可能です。

保存されるのはデータそのものではなく、参照されるテーブルのデータクラスの**キー**になります。

他のテーブルを参照するために必要な作業は主に以下の4つです。

* 参照されるテーブルのデータクラスへの `IRelationalData<T>` の実装
* 参照指定のための Attribute の実装
* カスタムセルの実装
* 参照するテーブルのフィールドへ Attribute を付与

int 値をキーとするテーブルを例に実装の流れを追ってみましょう。

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

#### データクラスへの IRelationalData の実装

あるテーブルを他のテーブルから参照するためには、テーブルのデータクラスに `IRelationalData<T>` を実装する必要があります。

`T` で指定した型がそのデータクラスのキーとなります。

また、DisplayName で生成した文字列がセルに表示される文字列となります。

参照されるテーブルは ProjectSettings/NonNonSheet の Tables に登録されている必要があります。テーブルアセットは作成時点で自動登録されますが、セルに「Table not found」と表示された場合はテーブルアセットが登録されているかを確認してください。\
また、同じデータクラスを使用したテーブルが複数登録されている場合、登録順の早いものが参照されるため注意してください。

```csharp
public class SampleTableRefAttribute : CellCustomAttribute { }
```

#### 参照指定のための Attribute の実装

`CellCustomAttribute` を継承した Attribute を実装します。命名に特に制約はありません。

ここで定義した Attribute がテーブル参照用フィールドを示す属性として使用されます。

```csharp
#if UNITY_EDITOR
[NonNonCell]
public class SampleTableRelationCell : DataRelationCell<int, SampleTableRefAttribute, SampleData> { }
#endif
```

#### カスタムセルの実装

`DataRelationCell` を継承したクラスを実装します。

`DataRelationCell` の型引数は

* 第１型引数: `IRelationalData<T>` の T と同じ型
* 第２型引数: `CellCustomAttribute` を継承した Attribute クラス
* 第３型引数: `IRelationalData<T>` を実装したデータクラス

また、このクラスは Editor only なアセンブリに配置するか、`#if UNITY_EDITOR` で囲む必要があるため注意してください。

```csharp
public class MultiSampleTableRefAttribute : CellCustomAttribute { }

#if UNITY_EDITOR
[NonNonCell]
public class SampleTableMultiRelationCell : MultiDataRelationCell<int, MultiSampleTableRefAttribute, SampleData> { }
#endif
```

`DataRelationCell` の代わりに `MultiDataRelationCell` を継承したクラスを実装することで、複数のキーを保存させることもできます。

<div align="left"><img src="~/images/image (1).png" alt=""></div>

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

#### 参照するテーブルのフィールドへ Attribute を付与

参照されるテーブルとは別のテーブルを実装します。

データクラスに `IRelationalData<T>` の T と同じ型のフィールドを実装しましょう。

そのフィールドに `CellCustomAttribute` を継承した Attribute（今回だと `SampleTableRefAttribute` または `MultiSampleTableRefAttribute`）を付与します。

<img src="~/images/image.png" alt="">

以上で ReferencingTable から SampleTable を参照することができるようになりました。

今回の例では int 値をキーにしましたが、 string や enum をキーとすることも可能です。

