# テーブルの基本

NonNonTable は、データ（Serializable なクラス/構造体）を List として保持する ScriptableObject です。エディタ上で表形式にレンダリングされ、各フィールドがセルとして編集可能になります。

<mark style="color:$danger;">**重要: NonNonSheet は「public フィールド」のみをサポートします。プロパティ（get/set）や SerializeField の付いた private フィールドは対象外です**</mark>

### 基本仕様

* データ型には `Serializable` が必須
* テーブルは ScriptableObject アセットとして保存
* 行の追加/削除、並び替えをサポート
* 列（セル）はデータ型の public フィールドに対応（ただし、サポートする型のみ）

### サポートする型

* C# プリミティブ: `bool`, `int`, `long`, `uint`, `ulong`, `float`, `double`, `string`
* 列挙体: `enum`（通常/Flags 両方）
* Unity 構造体: `Vector2`, `Vector3`, `Vector4`, `Vector2Int`, `Vector3Int`, `Rect`, `Bounds`, `Color`
* レイヤー関連: `LayerMask`, レイヤー番号（`int`。`[Layer]` 属性で UI 指定可）
* UnityEngine.Object 参照: `UnityEngine.Object` 参照全般（`GameObject`, `MonoBehaviour`, `ScriptableObject`, `Texture` など）
* Addressables: `AssetReference`（Addressables が有効なときのみ）
* 参照セル（リレーション）: 別テーブルのキー型（例: `int`, `string`）を参照するセル（詳細は後述）
