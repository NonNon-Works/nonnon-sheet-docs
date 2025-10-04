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

# Refer to other tables

{% columns %}
{% column width="50%" %}
<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column width="50%" %}
You can also enable users to select data from other tables through a dropdown menu.

What gets saved is not the actual data itself, but rather the key of the data class for the referenced table.

The primary requirements for referencing another table are as follows:

* Implementing IRelationalData for the data class of the referenced table
* Implementing the necessary Attribute for referencing
* Implementing a custom cell
* Applying the Attribute to the fields in the referenced table

Let's walk through the implementation process using an example of a table with integer-keyed data.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
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
{% endcolumn %}

{% column %}
### Implementing IRelationalData for the data class of the referenced table

To establish a foreign key relationship between tables, you must implement the IRelationalData interface in the data class of the target table.

The type specified by T serves as the key for that data class.

The string generated using DisplayName will be the text displayed in the cell.

{% hint style="info" %}
The referenced table must be listed in the Tables section of ProjectSettings/NonNonSheet. While table assets are automatically registered during creation, if the cell displays "Table not found," please verify that the table asset is properly registered.\
Additionally, note that if multiple tables using the same data class are registered, the one registered earlier will be referenced, so be aware of this order dependency.
{% endhint %}
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class SampleTableRefAttribute : CellCustomAttribute { }
```
{% endcolumn %}

{% column %}
### Implementing the necessary Attribute for referencing

Implement an Attribute that inherits from CellCustomAttribute.&#x20;

There are no specific naming constraints.

The Attribute defined here will be used to indicate fields intended for table referencing.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
#if UNITY_EDITOR
[NonNonCell]
public class SampleTableRelationCell : DataRelationCell<int, SampleDataRelationAttribute , SampleData> { }
#endif
```
{% endcolumn %}

{% column %}
### Implementing a custom cell

Implement a class that inherits from DataRelationCell.

The type parameters for DataRelationCell should be:

* First type parameter: The same type as T in IRelationalData
* Second type parameter: An Attribute class that inherits from CellCustomAttribute
* Third type parameter: A data class that implements IRelationalData

Note that this class must be placed in an Editor-only assembly or wrapped with #if UNITY\_EDITOR directives.
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public class MultiSampleTableRefAttribute : CellCustomAttribute { }

#if UNITY_EDITOR
[NonNonCell]
public class SampleTableMultiRelationCell : MultiDataRelationCell<int, MultiSampleTableRefAttribute, SampleData> { }
#endif
```
{% endcolumn %}

{% column %}
You can also implement a class that inherits from MultiDataRelationCell instead of DataRelationCell to store multiple keys.

<div align="left"><figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure></div>
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
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
{% endcolumn %}

{% column %}
### Applying the Attribute to the fields in the referenced table

Implement a separate table that references another table.

Implement a field in the data class with the same type as the T in IRelationalData.

Apply an Attribute that inherits from CellCustomAttribute to this field (in this case, either SampleTableRefAttribute or MultiSampleTableRefAttribute).
{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
With this setup, you can now reference SampleTable from ReferencingTable.

In this example we used an integer value as the key, but it's also possible to use strings or enumerations as keys.


{% endcolumn %}
{% endcolumns %}

