# カスタムセルを自作する

<img src="~/images/image (24).png" alt="">

カスタムセルはユーザーが実装・追加することも可能です。

そのために必要な作業は主に以下の2つです。

* カスタムセルを指定するための Attribute の実装
* カスタムセルの実装

```csharp
public class SampleCustomAttribute : CellCustomAttribute { }
```

#### カスタムセルを指定するための Attribute の実装

データクラスのフィールドに対し、カスタムセルの使用を指定するための属性を定義します。

この属性はテーブルのデータクラスで利用するため、テーブルやデータクラスと同じアセンブリに実装してください。

```csharp
[NonNonCell]
public class SampleCustomCell : CustomCell<[任意の型], SampleCustomAttribute>
{
    protected override void OnInitialize() { }
    
    protected override void OnBindProperty() { }
    
    public override void OnStartEditing() { }
}
```

#### カスタムセルの実装

次に `CustomCell` のサブクラスを実装し、 `NonNonCell` 属性を付与します。

`CustomCell` は `VisualElement` のサブクラスです。\
このクラスに `IntegerField` などを追加することで UI を表示することができます。

`CustomCell` の型引数は

* 第１型引数: セルで扱う値の型
* 第２型引数: 先ほど実装した `CellCustomAttribute` のサブクラス

CustomCell には3つのコールバックメソッドが用意されています。

* OnInitialize
  * セル生成時に呼び出されます
  * セルへの VisualElement 追加処理等を実装しましょう
* OnBindProperty
  * セルの生成やデータ並び替えなど、セルに紐づくデータが変更され、Binding 処理が必要なときに呼び出されます
  * OnInitialize で生成した VisualElement の Binding 処理を実装しましょう
* OnStartEditing
  * セル編集開始操作（セルをダブルクリック or セル選択中にF2またはEnterを入力）時に呼び出されます
  * 編集中のみ UI を変更したい場合、UI 変更処理を実装しましょう

また、カスタムセルは必ず Editor only なアセンブリに実装する必要があるため注意してください。

## カスタムセルの実装例

<img src="~/images/image (26).png" alt="">

日付入力用セルを例にカスタムセルの実装例を紹介します。

```csharp
[Serializable]
public class Date
{
    public int Year;
    public int Month;
    public int Day;
}
```

まず日付データ用のクラスを実装します。

`Date` 型のフィールドを編集するためのカスタムセルを実装していきます。

```csharp
public class DateAttribute : CellCustomAttribute { }
```

Attribute を実装します。

```csharp
#if UNITY_EDITOR
[NonNonCell]
public class DateCell : CustomCell<Date, DateAttribute> { }
#endif
```

カスタムセルのクラスを実装します。

これで最低限の準備は完了です。\
`Date` 型のフィールドに `DateAttribute` をつけると `DateCell` が生成されるようになりました。

```csharp
#if UNITY_EDITOR
[NonNonCell]
public class DateCell : CustomCell<Date, DateAttribute>
{
    private Label _yearLabel;
    private Label _monthLabel;
    private Label _dayLabel;
    private IntegerField _yearField;
    private IntegerField _monthField;
    private IntegerField _dayField;

    private bool _isEditing;

    protected override void OnInitialize()
    {
        _yearLabel = new Label();
        _monthLabel = new Label();
        _dayLabel = new Label();
        _yearField = new IntegerField();
        _monthField = new IntegerField();
        _dayField = new IntegerField();

        Add(_yearLabel);
        Add(_monthLabel);
        Add(_dayLabel);

        _yearField.RegisterCallback<FocusOutEvent, DateCell>((_, cell) => cell.EndEditing(), this);
        _monthField.RegisterCallback<FocusOutEvent, DateCell>((_, cell) => cell.EndEditing(), this);
        _dayField.RegisterCallback<FocusOutEvent, DateCell>((_, cell) => cell.EndEditing(), this);

        style.flexDirection = FlexDirection.Row;
        _yearLabel.style.flexGrow = 1;
        _monthLabel.style.flexGrow = 1;
        _dayLabel.style.flexGrow = 1;
        _yearLabel.style.width = new StyleLength(new Length(50, LengthUnit.Percent));
        _monthLabel.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
        _dayLabel.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
        _yearField.style.flexGrow = 1;
        _monthField.style.flexGrow = 1;
        _dayField.style.flexGrow = 1;
        _yearField.style.width = new StyleLength(new Length(50, LengthUnit.Percent));
        _monthField.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
        _dayField.style.width = new StyleLength(new Length(25, LengthUnit.Percent));
    }

    protected override void OnBindProperty()
    {
        _yearLabel?.BindProperty(FieldSerializedProperty.FindPropertyRelative("Year"));
        _monthLabel?.BindProperty(FieldSerializedProperty.FindPropertyRelative("Month"));
        _dayLabel?.BindProperty(FieldSerializedProperty.FindPropertyRelative("Day"));
        _yearField?.BindProperty(FieldSerializedProperty.FindPropertyRelative("Year"));
        _monthField?.BindProperty(FieldSerializedProperty.FindPropertyRelative("Month"));
        _dayField?.BindProperty(FieldSerializedProperty.FindPropertyRelative("Day"));
    }

    public override void OnStartEditing()
    {
        if (_isEditing) return;

        _isEditing = true;

        Clear();
        Add(_yearField);
        Add(_monthField);
        Add(_dayField);

        AddToClassList("input-cell"); // input-cell を追加しておくと編集中セルとしていい感じにスタイルが調整されます
    }

    private void EndEditing()
    {
        if (!_isEditing) return;

        Clear();
        Add(_yearLabel);
        Add(_monthLabel);
        Add(_dayLabel);

        RemoveFromClassList("input-cell");

        _isEditing = false;

        FocusNextFrame(); // 次のフレームに Focus() を実行します
    }
}
#endif
```

最後に各コールバックメソッドを用いて `DateCell` を実装して完了です。

