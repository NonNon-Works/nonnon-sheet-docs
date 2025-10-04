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

# How to use custom cells

You can customize the cell UI by adding specific attributes to fields.\
For example, adding the Slider attribute to a float type field will change the cell UI to a slider.

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



The custom cells implemented as standard are as shown in the table below.

| Attribute    | Type    |
| ------------ | ------- |
| Slider       | float   |
| IntSlider    | int     |
| MinMaxSlider | Vector2 |
| Tag          | string  |
| Layer        | int     |

Please note that if you assign these attributes to a field of the mismatch type, the cell will not function properly.
