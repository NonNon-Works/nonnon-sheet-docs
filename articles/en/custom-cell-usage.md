# Using Custom Cells

You can customize a field's cell UI by applying specific attributes. For example, applying the `Slider` attribute to a `float` field changes its cell UI to a slider.

<img src="~/images/image (28).png" alt="">

```csharp
[Serializable]
public class SampleData
{
    public float Float;
    [Slider(0f, 1f)] public float SliderFloat;
}
```

The following built-in attributes provide custom cell UIs:

| Attribute      | Type     |
| -------------- | -------- |
| `Slider`       | float    |
| `IntSlider`    | int      |
| `MinMaxSlider` | Vector2  |
| `Tag`          | string   |
| `Layer`        | int      |

If you apply one of these attributes to an incompatible field type, the custom cell will not function properly.
