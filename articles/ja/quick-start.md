# クイックスタート

1.  ダウンロード

    Unity アセットストアから NonNonSheet をダウンロード＆インポートします。
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

    Table を定義すると、Table アセットを Assets/Create/NonNonSheet から作成できるようになります。Assets/Create/NonNonSheet/SampleTable から Table アセットを作成します。
4.  Table を開く

    Table アセットのインスペクタから Open Sheet Window ボタンをクリックするか、Table アセットをダブルクリックするとウィンドウが開きます。

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>
