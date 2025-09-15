# クイックスタート

1.  ダウンロード

    Unity アセットストアから NonNon Sheet をダウンロード＆インポートします
2.  Databaseの作成

    Assets/Create/NonNon Sheet/NonNonDatabase から Database アセットを作成します。

    ProjectSettings 内の NonNon Sheet を開き、Database のフィールドに作成した Database アセットを設定します。
3.  Tableの定義

    SampleTable というスクリプトを作成します。その後、以下のように編集します。

    ```csharp
    using System;
    using NonNonSheet;
    using UnityEngine;

    [NonNonTable]
    public partial class SampleTable : NonNonTable<SampleData> { }

    [Serializable]
    public class SampleData
    {
        public int Id;
        public string Name;
    }
    ```
4.  Table アセットの作成

    Table を定義すると、その Table アセットを Assets/Create/NonNon Sheet から作成できるようになります。Assets/Create/NonNon Sheet/SampleTable から Table アセットを作成します。
5.  Tableを開く

    Table アセットのインスペクタから “Open Sheet Window” ボタンをクリックするか、Tableアセットをダブルクリックするとウィンドウが開きます。
