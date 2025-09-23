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

# データをバリデーションする

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
マスターデータの管理にあたって、データに制約を設けることはよくあることです。

NonNonSheet では問題のあるデータを素早く検知するため、セル単位でデータのバリデーションが可能です。


{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
[NonNonTable]
public partial class SampleTable : NonNonTable<SampleData>, ISampleTableValidator { }

[Serializable]
public class SampleData
{
    public int Id;
    public string Name;
}
```
{% endcolumn %}

{% column %}
バリデーションをするためには `Table` クラスに `IXxxValidator` インタフェースを実装します。


{% endcolumn %}
{% endcolumns %}



{% columns %}
{% column %}
```csharp
public interface ISampleTableValidator : IValidator<SampleData>
{
    ValidationResult ValidateId(SubclassData self, int id) => ValidationResult.Success();
    ValidationResult ValidateName(SubclassData self, string id) => ValidationResult.Success();
}
```
{% endcolumn %}

{% column %}
`IXxxValidator` インタフェースは SourceGenerator によって自動生成されます。\
例えば `SampleTable` クラスを定義すると、 `ISampleTableValidator` インタフェースが生成されます。

`IXxxValidator` インタフェースは Table クラスが持つ各フィールドに対するバリデーションメソッドが定義されています。\
それらのメソッドを実装することでデータのバリエーションを行います。

バリデーションメソッドは引数としてバリエーション対象のデータ全体と更新後のフィールドの値が渡されます。また、返値として `ValidationResult` を返すことでバリエーションの結果を


{% endcolumn %}
{% endcolumns %}



## バリデーションの例

{% columns %}
{% column %}
```csharp
[NonNonTable]
public partial class ValidationSampleTable : NonNonTable<ValidationSampleData>, IValidationSampleTableValidator
{
    ValidationResult IValidationSampleTableValidator.ValidateId(ValidationSampleData self, int id)
    {
        var sameIdData = Data.Where(v => v != self && v.Id == id).Cast<object>().ToArray();
        if (sameIdData.Any()) return ValidationResult.Error("duplicate ID", sameIdData);
        return ValidationResult.Success();
    }

    ValidationResult IValidationSampleTableValidator.ValidateName(ValidationSampleData self, string v)
    {
        if (string.IsNullOrEmpty(v)) return ValidationResult.Error("name must be non-empty");
        return ValidationResult.Success();
    }
}

[Serializable]
public class ValidationSampleData
{
    public int Id;
    public string Name;
}
```
{% endcolumn %}

{% column %}
`ValidationSampleTable` を例にいくつかのバリエーションの例を示します。


{% endcolumn %}
{% endcolumns %}



{% columns fullWidth="true" %}
{% column %}
```csharp
ValidationResult IValidationSampleTableValidator.ValidateName(ValidationSampleData self, string v)
{
    if (string.IsNullOrEmpty(v)) return ValidationResult.Error("name must be non-empty");
    return ValidationResult.Success();
}
```


{% endcolumn %}

{% column %}
まずはシンプルなバリエーションの例です。

`ValidateName` は `Name` フィールドが空の場合にエラーとするバリデーションメソッドです。

エラーの場合は `ValidationResult.Error` を返します。`ValidationResult.Error` の引数にメッセージを渡すことで、セルの `Tooltip` としてメッセージを表示することができます。

<div align="left"><figure><img src=".gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure></div>
{% endcolumn %}
{% endcolumns %}



{% columns fullWidth="true" %}
{% column %}
```csharp
ValidationResult IValidationSampleTableValidator.ValidateId(ValidationSampleData self, int id)
{
    var sameIdData = Data.Where(v => v != self && v.Id == id).Cast<object>().ToArray();
    if (sameIdData.Any()) return ValidationResult.Error("duplicate ID", sameIdData);
    return ValidationResult.Success();
}
```
{% endcolumn %}

{% column %}
次に重複チェックのバリデーションの例です。

`ValidateId` は重複する `Id` を持つデータが存在する場合にエラーとするバリデーションメソッドです。

`Table` クラスは `Data` プロパティに全データが格納されています。他データと `Id` を比較することで重複するものがないかどうかバリエーションすることができます。

重複するデータを `ValidationResult.Error` の第二引数として渡すことで、それらのデータもエラーとすることが可能です。
{% endcolumn %}
{% endcolumns %}







