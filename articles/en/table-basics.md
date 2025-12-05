# Table Basics

### Basic specifications

`NonNonTable` is a `ScriptableObject` that stores a `List<T>` of serializable row objects.<br>
It is rendered in a tabular format in the Unity **Editor**, and each field is editable on a per-cell basis.

<img src="~/images/image (6).png" alt=""><br>

The cell changes its editor/control based on the field type.

For example, an **IntegerField** is used for an `int` field, and a **Toggle** is used for a `bool` field.

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
* `[Flags] enum`
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

</details><br>

<img src="~/images/image (16).png" alt=""><br>

If a type does not have a dedicated cell, the value is shown as the string returned by `ToString()`.

Double-clicking a cell opens a popup with a `PropertyField`, allowing you to edit the value.

<img src="~/images/image (17).png" alt=""><br>

For arrays, the cell shows a comma-separated string of elements.

Double-clicking an array opens a popup to edit its contents.

### Restriction

* The table class **must be declared `partial`.**
* The generic type parameter `<T>` **must be a class; structs are not supported.**
