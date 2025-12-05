# Refer to other tables

<img src="~/images/image (18).png" alt=""><br>

You can allow users to select data from another table via a dropdown.

What is stored is not the full data object but the key (identifier) of the referenced table's data row.

To reference another table, you need to:

* Implement `IRelationalData<T>` on the data class of the table you want to reference
* Define an attribute used to mark referencing fields
* Implement a custom relation cell (`DataRelationCell` or `MultiDataRelationCell`)
* Apply the attribute to fields in the referencing table's data class

Below is an implementation example using an integer key.

```csharp
[NonNonTable]
public partial class SampleTable : NonNonTable<SampleData> { }

[Serializable]
public class SampleData : IRelationalData<int> 
{ 
    public int Key;
    public string Name;

    int IRelationalData<int>.Id => Key;
    string IRelationalData<int>.DisplayName => $"{Key}: {Name}";
}
```

#### Implement `IRelationalData` on the data class of the referenced table

To establish a foreign-key-like relationship, implement the `IRelationalData<T>` interface on the data class of the table being referenced.

* `T` is the key type for the data class.
* The value returned by `DisplayName` is shown in the dropdown cell.

The referenced table must be listed in the Tables section of `ProjectSettings/NonNonSheet`. Table assets are normally registered automatically when created. If the cell displays "Table not found," verify that the table asset is registered.\
If multiple tables using the same data class are registered, the one registered first will be used. Be aware of this order dependency.

```csharp
public class SampleTableRefAttribute : CellCustomAttribute { }
```

#### Define an attribute for referencing

Create an attribute that inherits from `CellCustomAttribute`.

There are no naming constraints.

Use this attribute to mark fields that reference another table.

```csharp
#if UNITY_EDITOR
[NonNonCell]
public class SampleTableRelationCell : DataRelationCell<int, SampleTableRefAttribute, SampleData> { }
#endif
```

#### Implement a custom relation cell

Implement a class that inherits from `DataRelationCell`.

Type parameters of `DataRelationCell<TKey, TAttribute, TData>`:

* `TKey`: Same type as `T` in `IRelationalData<T>`
* `TAttribute`: Attribute inheriting from `CellCustomAttribute`
* `TData`: Data class implementing `IRelationalData<TKey>`

Place this class in an editor-only assembly or wrap it with `#if UNITY_EDITOR`.

```csharp
public class MultiSampleTableRefAttribute : CellCustomAttribute { }

#if UNITY_EDITOR
[NonNonCell]
public class SampleTableMultiRelationCell : MultiDataRelationCell<int, MultiSampleTableRefAttribute, SampleData> { }
#endif
```

You can inherit from `MultiDataRelationCell` instead of `DataRelationCell` to store multiple keys.

<img src="~/images/image (1).png" alt=""><br>

```csharp
[NonNonTable]
public partial class ReferencingTable : NonNonTable<ReferencingData> { }

[Serializable]
public class ReferencingData
{
    [SampleTableRef] public int SampleTableKey;
    [MultiSampleTableRef] public int[] SampleTableKeys;
}
```

#### Apply the attribute to fields in the referencing table

Create a separate table that references the earlier table.

Add a field whose type matches the key type `T` used by the referenced data class (or an array when using `MultiDataRelationCell`).

Apply the attribute you defined (in this case `SampleTableRef` or `MultiSampleTableRef`).

<img src="~/images/image.png" alt=""><br>

With this setup, `ReferencingTable` can reference entries in `SampleTable`.

This example uses an integer key, but string or enum key types are also supported.

