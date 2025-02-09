# MissingScriptType
コンポーネントやScriptableObjectで欠けているスクリプトの型を表示して、簡単に見つけられるようにします。

[![openupm](https://img.shields.io/npm/v/com.solidalloy.missing-script-type?label=openupm&registry_uri=https://package.openupm.com)](https://openupm.com/packages/com.solidalloy.missing-script-type/)

## インストール方法

### OpenUPMでインストール

[OpenUPM cli](https://github.com/openupm/openupm-cli#installation)をインストール後、次のコマンドを実行:

```openupm install com.solidalloy.missing-script-type```

または、scoped registryをmanifest.jsonに追加:

```json
  "scopedRegistries": [
    {
      "name": "package.openupm.com",
      "url": "https://package.openupm.com",
      "scopes": [
        "com.solidalloy",
        "com.openupm"
      ]
    }
  ],
  "dependencies": {
    "com.solidalloy.missing-script-type": "1.0.1"
  },
```

### パッケージマネージャーでインストール

1. Unityで **Project Settings** -> **Package Manager** を開く
2. 新しいスコープ付きレジストリを追加:
   - **Name**: package.openupm.com
   - **URL**: https://package.openupm.com
   - **Scope(s)**:
     - com.openupm
     - com.solidalloy
3. **Apply** をクリック
4. **Window** -> **Package Manager** を開く
5. **+** ボタン → *Add package from git URL*
6. **com.solidalloy.missing-script-type** を入力 → **Add**

## クイックスタート

スクリプトが欠損したMonoBehaviourやScriptableObjectでは、以下のように表示されます:

<img src=".images/missing-script-ui.png" alt="missing-script-ui" style="zoom:67%;" />

以前にアサインされていたスクリプトの型名が表示されます。※パッケージインストール前にスクリプトが失われたオブジェクトのみ有効

該当スクリプトが見つかった場合、復元ボタンが表示されます:

<img src=".images/missing-script-button.png" alt="missing-script-button" style="zoom:67%;" />

## カスタマイズ

型名の表示形式は **Project Settings/Packages/Missing Script Type** で変更可能:

![settings](.images/settings.png)

- **Short**: TestBehaviour
- **Full**（デフォルト）: DefaultNamespace.TestBehaviour
- **Full And Assembly**: DefaultNamespace.TestBehaviour, Assembly-CSharp

## 他プラグインとの連携

**Odin Inspector** と連携可能。スクリプトがロードされていれば通常表示、欠損時は型情報を表示。

[GenericUnityObjects](https://github.com/SolidAlloy/GenericUnityObjects) とも連携:

<img src=".images/generic-unity-objects.png" alt="generic-unity-objects" style="zoom:67%;" />

## カスタムインスペクターでの使用

`MissingScriptTypeEditor` を継承して実装可能:

```csharp
[CustomEditor(typeof(MyClass))]
public class MyCustomEditor : MissingScriptTypeEditor
{
    protected override void OnEnable()
    {
        base.OnEnable();
        // カスタム初期化処理
    }
    
    public override void OnInspectorGUI()
    {
        if (_missingScriptUtility.IsScriptLoaded())
        {
            // 通常のインスペクターUI
        }
        else
        {
            _missingScriptUtility.Draw();
        }
    }
}
```

または `MissingScriptTypeUtility` を直接使用:

```csharp
[CustomEditor(typeof(MyClass))]
public class MyCustomEditor : Editor
{
    private MissingScriptTypeUtility _missingScriptUtility;
    
    protected override void OnEnable()
    {
        _missingScriptUtility = new MissingScriptTypeUtility(serializedObject);
    }
    
    public override void OnInspectorGUI()
    {
        if (_missingScriptUtility.IsScriptLoaded())
        {
            // 通常のインスペクターUI
        }
        else
        {
            _missingScriptUtility.Draw();
        }
    }
}
```

`MissingScriptTypeEditor` を無効化するには、**Player/Scripting Define Symbols** に `DISABLE_MISSING_SCRIPT_EDITOR` を追加。
