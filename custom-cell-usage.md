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

# カスタムセルの使い方

フィールドに特定の属性をつけることでセルの UI をカスタマイズすることができます。\
例えば float 型のフィールドに Slider 属性をつけるとセルの UI がスライダーに変更されます。

```csharp
[Serializable]
public class SampleData
{
    public int Id;
    public string Name;
    [Slider(0, 200)] public float Height;
}
```

標準で実装されているカスタムセルは以下の表の通りです。

| 属性           | 型       |
| ------------ | ------- |
| Slider       | float   |
| IntSlider    | int     |
| MinMaxSlider | Vector2 |
| Tag          | string  |
| Layer        | int     |

型の合わないフィールドにこれらの属性を付与すると、セルが正常に動作しなくなるため注意してください。
