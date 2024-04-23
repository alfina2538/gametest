# GametestFrameworkの設定方法

## 目次

- [GametestFrameworkの設定方法](#gametestframeworkの設定方法)
  - [目次](#目次)
  - [まえがき](#まえがき)
  - [Yarnのインストール](#yarnのインストール)
  - [Visual Studio Codeのインストール](#visual-studio-codeのインストール)
  - [拡張機能のインストール](#拡張機能のインストール)
  - [manifest.jsonの用意](#manifestjsonの用意)
    - [uuid](#uuid)
    - [modulesのentry](#modulesのentry)
    - [dependenciesのversion](#dependenciesのversion)
  - [パッケージの初期化](#パッケージの初期化)
  - [モジュールのインストール](#モジュールのインストール)
  - [Typescriptの前準備](#typescriptの前準備)
  - [スクリプトを記述していく](#スクリプトを記述していく)
    - [ざっくり解説](#ざっくり解説)
    - [ざっくり解説その2](#ざっくり解説その2)
  - [TypescriptをJavascriptに変換する](#typescriptをjavascriptに変換する)
  - [ワールドで実行してみる](#ワールドで実行してみる)
  - [あとがき](#あとがき)

## まえがき

統合版minecraftについての説明です。

環境はWindows10/11を想定しています。

GametestFrameworkは`"javascript"`を用いて  
動作させる機能ですが、  
ここでは`"typescript"`を使って記述します。  
  
また、  
`"typescript"`の記述方法や記述ルールについて  
一切触れません。  
`"typescript"`について学びたい方は以下から。  
[サバイバル Typescript](https://typescriptbook.jp/)  
  
## Yarnのインストール

パッケージマネージャーは`"yarn"`使います。  

`"yarn"`をインストールするためには`Node.js`が必要です。  
以下からインストールしてください。  
[Node.js](https://nodejs.jp/)

`"yarn"`のインストールは  
以下のページを参考にしてください。  
[Installation Yarn](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable)

## Visual Studio Codeのインストール

`GametestFramework`のコードを編集するエディタは  
`Visual Studio Code`(以下VSCode)を使用します。
  
テキスト編集ができるエディタであればいいので  
VSCode以外がいい場合は好きなエディタを使ってください。
  
以下からインストールできます  
[Visual Studio Code](https://code.visualstudio.com/)

## 拡張機能のインストール

VSCodeでGametestFrameworkを記述していくうえで  
便利な拡張機能をインストールします。  

VSCodeを起動して`□が4つ集まったボタン`を  
押してください。

開いた画面で以下の拡張機能を検索してインストールします。

- [Blockception's Minecraft Bedrock Development](https://marketplace.visualstudio.com/items?itemName=BlockceptionLtd.blockceptionvscodeminecraftbedrockdevelopmentextension)
- [TypeScript Importer](https://marketplace.visualstudio.com/items?itemName=pmneo.tsimporter)

※あると便利

- [TypeScript Import Sorter](https://marketplace.visualstudio.com/items?itemName=mike-co.import-sorter)

## manifest.jsonの用意

`"manifest.json"`はminecraftのワールドに対して  
ビヘイビアパックやリソースパックを適用したとき  
これこれこういう機能を使って  
このデータを使うよと知らせるためのデータです。

VSCodeで任意のフォルダを開きます。  
(スクリプト書く用に新規で作るのがいいかも)  

VSCodeのエクスプローラーからファイルを追加して  
`"manifest.json"`というファイルを作成してください。

`"manifest.json"`ファイルには  
以下のように記述します。  

```json
{
    "format_version": 2,
    "header": {
        "name": "pack name(パックの名前)",
        "description": "pack description(パックの説明)",
        "uuid": "ccbc7b61-bf8f-4419-99b5-a526a730730c",
        "version": [1, 0, 0],
        "min_engine_version": [1, 20, 0]
    },
    "modules": [
        {
            "type": "script",
            "uuid": "ccbc7b61-bf8f-4419-99b5-a526a730730c",
            "description": "module description(モジュールの説明)",
            "language": "javascript",
            "entry": "scripts/js/main.js",
            "version": [1, 0, 0]
        }
    ],
    "capabilities": ["script_eval"],
    "dependencies": [
        {
            "module_name": "@minecraft/server",
            "version": "1.10.0-beta"
        }
    ]
}
```

### uuid

`"uuid"`は同名のパッケージが読み込まれても、  
それぞれ別のものだと認識させるためのデータです。  
なので、`"uuid"`には別の値を入力してください。  

uuidを自動生成してくれるサイトもあるので  
活用することをお勧めします。

### modulesのentry

ここに記入されたjsファイルを起点にして  
処理が開始されます。

### dependenciesのversion

GametestFrameworkの`"@minecraft/server"`モジュールは  
頻繁にバージョンが変わります。  
その時の最新がどのバージョンなのか  
調べてから記入してください。

## パッケージの初期化

VSCode上で`Ctrl + @`キーを押してターミナルを開きます。  
そこで以下を記入します。  

```term
yarn init
```

何かいろいろ出てきますが  
Enter押しまくっていきます。  
`package.json`ファイルができていれば完了です。

## モジュールのインストール

スクリプトを書き込んでいく前にモジュールをインストールします。  
VSCodeのターミナルで以下を記入して  
必要なモジュールをインストールします。  

```term
yarn add @minecraft/server
```

```term
yarn add typescript
```

package.jsonを開いて  
`"dependencies"`に以下があればインストール完了です。  
(バージョンの部分は状況によって変わります)

```json
"dependencies": {
    "@minecraft/server": "^1.9.0-beta.1.20.60-preview.26",
    "@minecraft/server-ui": "^1.1.0",
    "typescript": "^5.3.3"
}
```

## Typescriptの前準備

再度、VSCodeのターミナルで以下を記入します。  

```term
yarn tsc init
```

`"tsconfig.json"`ファイルができます。  
`"tsconfig.json"`ファイルの中身をすべて消して  
以下に書き換えます。  

```json
{
  "compilerOptions": {
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ES2020",
    "moduleResolution": "Node",
    "target": "ES2021",
    "lib": ["ES2021", "DOM"],
    "allowSyntheticDefaultImports": true,
    "noImplicitAny": true,
    "preserveConstEnums": true,
    "sourceMap": false,
    "rootDir": "scripts/ts",
    "outDir": "scripts/js"
  },
  "include": ["scripts/ts", "node_modules/@minecraft/server/index.d.ts"],
  "exclude": ["scripts/js"]
}
```

さて、ここまでで準備は完了です。

------

## スクリプトを記述していく

VSCodeのエクスプローラーで「scripts」フォルダを作成します。  
さらに「scripts」フォルダの中に  
「ts」フォルダと「js」フォルダを作成してください。  

次に「ts」フォルダの中に「main.ts」というファイルを作成します。
階層は以下の感じになると思います。

```folder
pack_folder
┠node_modules
┠scripts
┃┠ts
┃┃┗main.ts
┃┗js
┠manifest.json
┠package.json
┠tsconifg.json
┗yarn.lock
```

さてここからスクリプトを記述していきます。  
`"main.ts"`を開きます。

今回はプレイヤーがスニークしたとき  
透明化するスクリプトを組みます。

まずは「毎ティックプレイヤーがスニークしているか」  
を検知します。

```typescript
import { system, world } from "@minecraft/server";

system.runInterval(() => {
    world.getPlayers().forEach((player) => {
        if (player.isSneaking) {
            // スニークしているとき処理
        }
    });
}, 1);
```

### ざっくり解説

**`system.runInterval(call_back, par_tick)`**  
`call_back`に書かれている処理を  
`par_tick`毎に処理します。

**`world.getPlayers().forEach(call_back)`**  
`world.getPlayers()`でワールドにいる  
プレイヤーすべてを取得します。  
戻り値は`Player[]`というクラスの配列になっているので  
`.forEach`で1人ずつ処理しています。  

ここからプレイヤーに対して  
透明化の処理を入れます。  

```typescript
import { system, world } from "@minecraft/server";

system.runInterval(() => {
    world.getPlayers().forEach((player) => {
        if (player.isSneaking) {
            player.addEffect("invisibility", 0, {amplifier: 1, showParticles: false});
        }
    });
}, 1);
```

### ざっくり解説その2

**`player.addEffect(effect_name, duration, option)`**  

プレイヤーに対してeffectを付与します。  
`effect_name`にはコマンドで使用しているのと同じ  
エフェクトの名前を記入します。  

`duration`はエフェクトが  
適用されるまでの時間（ティック）です。

`option`はエフェクトの詳細を設定します。  
`amplifier`はエフェクトの強さ  
`showParticles`はエフェクトがかかっているとき  
パーティクルを表示するかしないかです。

## TypescriptをJavascriptに変換する

スクリプトが一通り記述できたら  
`"typescript"`のファイルを  
`"javascript"`に変換します。

VSCodeのターミナルを開き以下を入力します。

```term
yarn tsc
```

すると`scripts/js`フォルダの中に  
`main.js`というファイルができます。

これで変換は完了です。

## ワールドで実行してみる

minecraftで新規ワールドを作成します。
`実験`タブの  
`今後のクリエイター機能`と  
`ベータAPI`のチェックをONにしてください。

また、
`チート`タブの  
`チート`のチェックを入れておいてください。

設定が終わったらワールドを作成します。  
作成が終わったらいったんminecraftを終了してください。

エクスプローラーから  
以下の場所を開きます。  
`C:\Users\[ユーザー名]\AppData\Local\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\minecraftWorlds`  
※ここまで到達がめんどくさいのでショートカットを作成しておくことをお勧めします。

ここに統合版minecraftで作成した  
ワールドデータが格納されています。

先ほど作ったワールドデータを探し、  
`behavior_packs`フォルダの中に  
先ほど作ったパックをフォルダごとコピーして入れます。
(今回だと`pack_folder`まるごとコピーです)

コピーが完了したら再度  
minecraftを起動します。

----;

minecraftが起動したら、  
先ほど作成したワールドデータの設定を開きます。

`ビヘイビアーパック`の項目を選択し、  
`マイパック`から自分が作成したパックを有効化してください。

有効化できたらワールドを開きます。

プレイヤーをスニークさせると  
透明になると思います。

## あとがき

GametestFrameworkでは様々なことができます。  
ぜひともスクリプトを用いて  
オリジナルのゲームを作ってみてください。

どんな事ができるかは  
以下にドキュメントがあるので  
そちらを見てみてください。  
[Script API Reference Documentation](https://learn.microsoft.com/ja-jp/minecraft/creator/scriptapi/?view=minecraft-bedrock-stable)
