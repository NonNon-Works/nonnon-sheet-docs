# クイックスタート

1.  ダウンロード

    Unity アセットストアから NonNon Sheet をダウンロード＆インポートします
2.  Table の定義

    SampleTable というスクリプトを作成し、以下のように編集します。

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
3.  Table アセットの作成

    Table を定義すると、その Table アセットを Assets/Create/NonNon Sheet から作成できるようになります。Assets/Create/NonNon Sheet/SampleTable から Table アセットを作成します。
4.  Table を開く

    Table アセットのインスペクタから “Open Sheet Window” ボタンをクリックするか、Tableアセットをダブルクリックするとウィンドウが開きます。

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

1.
2.
