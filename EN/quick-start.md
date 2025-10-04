# Quickstart

1.  download

    Download and import NonNon Sheet from the Unity Asset Store.
2.  Table implementation

    Create a script called `SampleTable` and edit it as follows:

    ```csharp
    using System;
    using NonNonSheet;

    [NonNonTable]
    public partial class SampleTable : NonNonTable<SampleData> { }

    [Serializable]
    public class SampleData
    {
        public int Id;
        public string Name;
    }
    ```
3.  Creating a Table asset

    Once you implement Table, you can create a Table asset from Assets/Create/NonNon Sheet. Create a Table asset from Assets/Create/NonNon Sheet/SampleTable.
4.  Open a Table

    Click the Open Sheet Window button in the Table asset's Inspector, or double-click the Table asset to open the window.

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>
