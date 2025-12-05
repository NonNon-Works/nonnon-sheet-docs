# データをバリデーションする

<img src="~/images/image (10).png" alt=""><br>

マスターデータの管理にあたって、データに制約を設けることはよくあることです。

NonNonSheet では問題のあるデータを素早く検知するため、セル単位でデータのバリデーションが可能です。

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

バリデーションをするためには `Table` クラスに `IXxxValidator` インタフェースを実装します。

```csharp
public interface ISampleTableValidator : IValidator<SampleData>
{
  ValidationResult ValidateId(SampleData self, int id) => ValidationResult.Success();
  ValidationResult ValidateName(SampleData self, string name) => ValidationResult.Success();
}
```

`IXxxValidator` インタフェースは SourceGenerator によって自動生成されます。\
例えば `SampleTable` クラスを定義すると、 `ISampleTableValidator` インタフェースが生成されます。

`IXxxValidator` インタフェースは Table クラスが持つ各フィールドに対するバリデーションメソッドが定義されています。\
それらのメソッドを実装することでデータのバリデーションを行います。

バリデーションメソッドには引数として「行データ全体（self）」と「編集後のフィールド値」が渡されます。返り値として `ValidationResult` を返すことでバリデーション結果（成功/エラー とエラーメッセージ、関連データ）をシートに伝えます。

## バリデーションの例

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

`ValidationSampleTable` を例にいくつかのバリデーションの例を示します。

```csharp
ValidationResult IValidationSampleTableValidator.ValidateName(ValidationSampleData self, string v)
{
    if (string.IsNullOrEmpty(v)) return ValidationResult.Error("name must be non-empty");
    return ValidationResult.Success();
}
```

まずはシンプルなバリデーションの例です。

`ValidateName` は `Name` フィールドが空の場合にエラーとするバリデーションメソッドです。

エラーの場合は `ValidationResult.Error` を返します。`ValidationResult.Error` の引数にメッセージを渡すことで、セルの `Tooltip` としてメッセージを表示することができます。

<img src="~/images/image (11).png" alt=""><br>

```csharp
ValidationResult IValidationSampleTableValidator.ValidateId(ValidationSampleData self, int id)
{
    var sameIdData = Data.Where(v => v != self && v.Id == id).Cast<object>().ToArray();
    if (sameIdData.Any()) return ValidationResult.Error("duplicate ID", sameIdData);
    return ValidationResult.Success();
}
```

次に重複チェックのバリデーションの例です。

`ValidateId` は重複する `Id` を持つデータが存在する場合にエラーとするバリデーションメソッドです。

`Table` クラスは `Data` プロパティに全データが格納されています。他データと `Id` を比較することで重複するものがないかどうかバリデーションすることができます。

重複するデータを `ValidationResult.Error` の第二引数として渡すことで、それらのデータもエラーとすることが可能です。

