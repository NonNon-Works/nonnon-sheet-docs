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

# カスタムセルを自作する

カスタムセルはユーザーが実装することも可能です。\
そのためには「カスタムセル自体」の実装と「カスタムセルを指定するための属性」の実装が必要になります。

{% columns %}
{% column %}
```csharp
public class SampleCustomAttribute : CellCustomAttribute { }
```
{% endcolumn %}

{% column %}
最初にデータクラスのフィールドでカスタムセルを指定するための属性を定義します。\
この属性はテーブルのデータクラスで利用するため、テーブルやデータクラスと同じアセンブリに実装してください。
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
```csharp
[NonNonCell]
public class SampleCustomCell : CustomCell<[任意の型], SampleCustomAttribute> { }
```
{% endcolumn %}

{% column %}
次に `CustomCell` のサブクラスを実装し、 `NonNonCell` 属性を付与します。\
`CustomCell` の第一型引数はそのセルで扱う値の型を、第二型引数には先ほど定義した属性を指定します。\
カスタムセルは必ず Editor only なアセンブリに実装してください。

`CustomCell` は `VisualElement` のサブクラスです。\
`CustomCell` に `IntegerField` などを追加することで UI を表示することができます。
{% endcolumn %}
{% endcolumns %}

### シンプルなカスタムセル

{% columns %}
{% column %}
<div align="left"><figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure></div>


{% endcolumn %}

{% column %}
今回は年月日を入力できるカスタムセルを試しに作ってみましょう。

最初にコード全体をお見せします。細部については後程説明いたします。

なお、このコードは Editor only なアセンブリに配置してください。
{% endcolumn %}
{% endcolumns %}

```csharp
using NonNonSheet.Editor;
using UnityEditor.UIElements;
using UnityEngine.UIElements;

[NonNonCell]
public class SampleCustomCell : CustomCell<SampleCustomCellData, SampleCustomAttribute>
{
    private IntegerField _yearField;
    private IntegerField _monthField;
    private IntegerField _dayField;

    protected override void OnInitialize()
    {
        _yearField = new IntegerField();
        _monthField = new IntegerField();
        _dayField = new IntegerField();
        Add(_yearField);
        Add(_monthField);
        Add(_dayField);

        _yearField.style.flexGrow = 1;
        _monthField.style.flexGrow = 1;
        _dayField.style.flexGrow = 1;
        _yearField.style.width = new StyleLength(new Length(50, LengthUnit.Percent));
        _monthField.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
        _dayField.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
        style.flexDirection = FlexDirection.Row;

        AddToClassList("input-cell");
    }

    public override void OnBindProperty()
    {
        _yearField?.BindProperty(CellSerializedProperty.FindPropertyRelative("Year"));
        _monthField?.BindProperty(CellSerializedProperty.FindPropertyRelative("Month"));
        _dayField?.BindProperty(CellSerializedProperty.FindPropertyRelative("Day"));
    }
}
```



{% columns %}
{% column %}
```csharp
protected override void OnInitialize()
{
    _yearField = new IntegerField();
    _monthField = new IntegerField();
    _dayField = new IntegerField();
    Add(_yearField);
    Add(_monthField);
    Add(_dayField);

    _yearField.style.flexGrow = 1;
    _monthField.style.flexGrow = 1;
    _dayField.style.flexGrow = 1;
    _yearField.style.width = new StyleLength(new Length(50, LengthUnit.Percent));
    _monthField.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
    _dayField.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
    style.flexDirection = FlexDirection.Row;

    AddToClassList("input-cell");
}
```
{% endcolumn %}

{% column %}
OnInitialize() は初期化処理を行うためのコールバックメソッドです。

セルが生成される最初の1回だけ呼ばれます。

ここで各種 UI を追加しましょう。必要であればスタイルの調整もここでやってしまいましょう。

{% hint style="info" %}
AddToClassList("input-cell") しておくと、各種の標準UIが入力中のスタイルに変更されます。編集可能なセルは常に AddToClassList("input-cell") しておくことをおすすめします。
{% endhint %}


{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public override void OnBindProperty()
{
    _yearField?.BindProperty(CellSerializedProperty.FindPropertyRelative("Year"));
    _monthField?.BindProperty(CellSerializedProperty.FindPropertyRelative("Month"));
    _dayField?.BindProperty(CellSerializedProperty.FindPropertyRelative("Day"));
}
```


{% endcolumn %}

{% column %}
BindData はセル中の UI とデータクラスのフィールドを紐づけるためのコールバックメソッドです。

セル生成時とデータの並び替え、削除などでデータのインデックスが変更されたときに呼ばれます。

UIとデータの紐付けはここで行いましょう。

{% hint style="info" %}
なぜUIとデータの紐付け処理だけ初期化処理とは別にコールバックが用意されているのかと疑問に思ったかもしれません。

これらが分離されている理由はセルに紐づけるべきデータのインデックスが変更された際に再度データの紐付けが必要になるためです。
{% endhint %}


{% endcolumn %}
{% endcolumns %}
