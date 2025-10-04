---
layout:
  width: default
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

# Using Custom Cells

You can customize a field's cell UI by applying specific attributes. For example, applying the `Slider` attribute to a `float` field changes its cell UI to a slider.

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
```csharp
[Serializable]
public class SampleData
{
    public float Float;
    [Slider(0f, 1f)] public float SliderFloat;
}
```
{% endcolumn %}
{% endcolumns %}



The following built-in attributes provide custom cell UIs:

| Attribute      | Type     |
| -------------- | -------- |
| `Slider`       | float    |
| `IntSlider`    | int      |
| `MinMaxSlider` | Vector2  |
| `Tag`          | string   |
| `Layer`        | int      |

If you apply one of these attributes to an incompatible field type, the custom cell will not function properly.
