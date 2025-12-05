# Create custom cell

You can implement and register your own custom cells.

There are two main steps:

* Define an attribute that marks fields to use the custom cell
* Implement the custom cell itself

```csharp
public class SampleCustomAttribute : CellCustomAttribute { }
```

#### Create a Marker Attribute

Create a marker attribute that tells the system a field should be rendered using a custom cell.

This marker attribute must exist in the same assembly as the table or data class that uses it.

```csharp
[NonNonCell]
public class SampleCustomCell : CustomCell<TData, SampleCustomAttribute>
{
  protected override void OnInitialize() { }

  protected override void OnBindProperty() { }

  public override void OnStartEditing() { }
}
// TData: the data type handled by this cell
```

#### Implement a Custom Cell

Next, create a subclass of `CustomCell` and apply the `NonNonCell` attribute.

`CustomCell` derives from `VisualElement`. Add UI Toolkit controls (e.g., `IntegerField`) to build the cell interface.

Custom cells must always live in an editor-only assembly.

Type parameters of `CustomCell`:

* First: the data type handled by the cell
* Second: the `CellCustomAttribute` subclass you defined earlier

`CustomCell` exposes three callback methods:

* `OnInitialize`
  * Called once when the cell is created
  * Instantiate controls and add `VisualElement`s here
* `OnBindProperty`
  * Called when the underlying data / serialized property changes (e.g., initial generation or row reorder)
  * Bind the controls you created in `OnInitialize` here
* `OnStartEditing`
  * Called when editing begins (double‑click, F2, or Enter while selected)
  * Add or adjust UI that should appear only during editing here

## Custom Cell Implementation Example

<img src="~/images/image (26).png" alt=""><br>

The following example demonstrates a custom cell using a simple date type.

```csharp
[Serializable]
public class Date
{
    public int Year;
    public int Month;
    public int Day;
}
```

Define a class for date data.

```csharp
public class DateAttribute : CellCustomAttribute { }
```

Define the attribute.

```csharp
#if UNITY_EDITOR
[NonNonCell]
public class DateCell : CustomCell<Date, DateAttribute> { }
#endif
```

Implement the custom cell class.

This is the minimal setup.\
Applying `DateAttribute` to a `Date` field will now render using a `DateCell`.

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

        AddToClassList("input-cell"); // Adding this class applies editing styles to the cell
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

        FocusNextFrame(); // Calls Focus() on the next frame
    }
}
#endif
```

Finally, implement `DateCell` using each callback to complete the process. The cell swaps between display mode (labels) and edit mode (integer fields).
