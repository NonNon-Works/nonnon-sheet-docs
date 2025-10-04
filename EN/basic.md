# Table Basics

### Basic specifications

NonNonTable is a `ScriptableObject` that has a List of `Serializable` classes.\
It is rendered in tabular format in the Unity editor, allowing each field to be edited cell by cell.

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
The cell changes shape depending on the field type.

For example, a cell with IntegerField is generated for an int type field, and a cell with Toggle is generated for a boolean type field.

Dedicated cells are available for the following types.
{% endcolumn %}
{% endcolumns %}

<details>

<summary>Types with dedicated cells</summary>

* `bool`
* `sbyte`
* `byte`
* `short`
* `ushort`&#x20;
* `int`&#x20;
* `uint`&#x20;
* `long`&#x20;
* `ulong`&#x20;
* `float`
* `double`
* `string`
* `enum`
* `[Falges] enum`
* `Vector2`
* `Vector3`
* `Vector4`
* `Vector2Int`
* `Vector3Int`
* `Rect`
* `Bounds`
* `Color`
* `LayerMask`&#x20;
* `UnityEngine.Object`（`GameObject`, `MonoBehaviour`, `ScriptableObject`, `Texture`, etc.）
* `AssetReference` (only if Addressables is enabled)

</details>

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
If a type does not have a dedicated cell, the value will be displayed in the cell as a string obtained by ToString().

Also, double-clicking a cell will display a Popup with a PropertyField, allowing you to edit the value.


{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
In the case of an array, a string with each element separated by a comma will be displayed in the cell.

Double-clicking an array will display a popup that allows you to edit the contents.
{% endcolumn %}
{% endcolumns %}

### &#x20;Restriction

NonNonTable has some implementation restrictions.

* partial class
* Data type `<T>` is only class, struct is not supported
