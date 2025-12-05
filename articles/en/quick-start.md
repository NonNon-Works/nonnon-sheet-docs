# Quickstart

1.  Download

    Download NonNon Sheet from Unity Asset Store and import it into your Unity project.
2.  Implement a table class

    Create a new C# script named `SampleTable`, and define it as follows:

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
3.  Create a Table asset

    After implementing the table class, create a Table asset via:\
    `Assets → Create → NonNon Sheet → SampleTable`
4.  Open the sheet window

    To view or edit the table, use one of these methods:

    * Click the **Open Sheet Window** button in the Inspector of the Table asset
    * Double-click the Table asset in the Project window

    <br><img src="~/images/image (19).png" alt="">
