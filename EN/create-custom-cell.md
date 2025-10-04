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

# Create Custom Cells

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
Custom cells can also be implemented and added by users.

The necessary tasks primarily consist of the following two:

* Implementing Attribute for Custom Cell Specification
* Implementing a Custom Cell
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class SampleCustomAttribute : CellCustomAttribute { }
```
{% endcolumn %}

{% column %}
### Implementing Attribute for Custom Cell Specification

Defines an attribute to specify the use of custom cells for fields in data classes.

This attribute must be implemented in the same assembly as the table or data class it will be used with.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
[NonNonCell]
public class SampleCustomCell : CustomCell<[任意の型], SampleCustomAttribute>
{
    protected override void OnInitialize() { }
    
    protected override void OnBindProperty() { }
    
    public override void OnStartEditing() { }
}
```
{% endcolumn %}

{% column %}
### Implementing a Custom Cell

Next, implement a subclass of CustomCell and apply the NonNonCell attribute.

CustomCell is a subclass of VisualElement.\
You can display the UI by adding components like IntegerField to this class.

Note that custom cells must always be implemented in an Editor-only assembly, so be aware of this requirement.

The type parameters for CustomCell are:

* First type parameter: The data type handled by the cell
* Second type parameter: A subclass of the CellCustomAttribute implemented earlier

CustomCell provides three callback methods:

* OnInitialize
  * Called when the cell is initialized
  * Implement any necessary operations here, such as adding VisualElements to the cell
* OnBindProperty
  * Called when data associated with the cell changes (e.g., cell generation or data reordering), requiring binding operations
  * Implement the binding logic for the VisualElements created in OnInitialize here
* OnStartEditing
  * Called when cell editing begins (either by double-clicking the cell or pressing F2 or Enter while the cell is selected)
  * If you want to modify the UI only during editing, implement the UI change logic here
{% endcolumn %}
{% endcolumns %}



## Custom Cell Implementation Example

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
The following demonstrates a custom cell implementation using date input cells as an example.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
[Serializable]
public class Date
{
    public int Year;
    public int Month;
    public int Day;
}
```
{% endcolumn %}

{% column %}
Implement a class for date data.




{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class DateAttribute : CellCustomAttribute { }
```
{% endcolumn %}

{% column %}
Implement Attribute.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
#if UNITY_EDITOR
[NonNonCell]
public class DateCell : CustomCell<Date, DateAttribute> { }
#endif
```
{% endcolumn %}

{% column %}
Implement a custom cell class.

This completes the minimum setup.\
Now attaching DateAttribute to a Date type field will generate a DateCell.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
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

        AddToClassList("input-cell"); // input-cell を追加しておくと編集中セルとしていい感じにスタイルが調整されれます
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
{% endcolumn %}

{% column %}
Finally, implement DateCell using each callback method to complete the process.
{% endcolumn %}
{% endcolumns %}
