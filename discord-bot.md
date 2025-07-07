# Discord bot の作り方(起動編)

## 使用するもの

- **Node.js** : JavaScript を PC で実行するために必要なソフトウェア。Bot のエンジンのようなもの

> Node.js のダウンロード時は「LTS(推奨版)」を選ぼう！<br>
> 長期サポートされていて安定性が高く、初心者にも安心！

- **Visual Studio Code** : コードを書くためのエディタ。見やすく、便利な機能が多くておすすめ！

- **アカウント一覧** :
- Discord : Bot を動かしたりテストしたりするために必要！
- GitHub : 作ったコードを保存・共有する場所
- Render : Bot をオンラインで常時動かすためのホスティングサービス
- Google : Bot のスリープを防ぐ定期アクセスに GAS(Google Apps Script)を使う

---

## プロジェクトの初期設定

1. デスクトップにフォルダを作成(名前を my-discord-bot に)
1. VS Code でそのフォルダを開く
1. ターミナルを起動(`Ctrl + @` or `control + @`)し、コマンドを実行

   ```bash
   npm -init -y
   ```

   > npm init はプロジェクトの設定ファイル(package.json)を作成するコマンド。<br> > `-y`をつけると、質問をスキップして自動生成される！

1. パッケージをインストール

   ```bash
   npm install discord.js dotenv express
   ```

   > 各パッケージの用途 :
   >
   > - `discord.js` : Bot の本体を構築するライブラリ
   > - `dotenv` : `.env`に書いた機密情報を読み込むためのツール
   > - `express` : Render 上で Bot を動かすための軽量な Web サーバー

## プロジェクト構成

```
▼ commands/ (コマンドファイルを入れる場所)
├─ ▼ ping.mjs
└─ ▼ ...
▼ handlers/ (イベントの受け口)
└─ messageCreate.mjs
▼ models/ (データの定義)
└─ ...
.env (トークンなどの機密情報を保存)
.gitignore (Gitで無視したいファイルを指定)
main.mjs (Botの起動スクリプト)
package.json (使用パッケージの情報)
...
```

> ディレクトリをこのように整理しておくと、今後機能を追加したり管理しやすくなる！

1. 上記のようなファイルやフォルダを作成
1. `.gitignore`にコードを記入

   ```
   node_modules
   .env
   .DS_Store
   ```

   > Git に送りたくないファイルを書く！<br>
   > .env は特に絶対に誰にも見せてはいけないので書いておく！！！

1. `.env`にコードを記入

   ```
   # Botの認証トークン(絶対に漏らさない！！)
   DISCORD_TOKEN=xxxxx

   # アプリケーションID (招待URLやコマンド登録に使用)
   CLIENT_ID=123456789012345678
   ```

## アプリケーションを作成

1. **Discord Developer Portal** を開く
1. **New Application** を押す
1. Bot の名前を入力し、チェックボックスにチェックを入れて Create を押す
1. Bot の設定画面に飛ぶので、アイコンと説明文などを設定する
1. **APPLICATION ID** をコピーし、`.env`の`CLIENT_ID`に貼り付け
1. Save Changes を押して保存
1. **Installation** タブに移動し、Guild Install の SCOPES に bot を追加
1. 下の PERMISSIONS を今回は Administrator(管理者権限を付与)にしておく
1. Save Changes で保存
1. Install Link をコピーし、ブラウザで開く
1. Discord アプリが入っていれば勝手に飛んでくれるので、入れたい鯖に招待する
1. Bot タブに移動し、Privileged Gateway Intents の一番下 MESSAGE CONTENT INTENT を ON にする(ON にしないとメッセージの情報を取得できない)
1. Save Changes で保存
1. TOKEN の **Reset Token** を押す
1. 確認画面が出てくるので **Yes, do it!**
1. Authenticate with security key というボタンが出てくるので押す
1. 認証画面が出てくるので、認証する
1. トークンが表示されるので、コピーし、`.env`の`DISCORD_TOKEN`に貼り付け
   > このトークンは、リセットした直後しか見れないので、すぐにコピーして貼り付けておく！

## Bot を起動する

`main.mjs`に以下をコピペ

```javascript
// main.mjs

// インストールしたパッケージをここで使えるようにインポートする！
import { Client, GatewayIntentBits } from 'discord.js';
import dotenv from 'dotenv';
import express from 'express';

// .envファイルから環境変数を読み込むためにdotenvを設定する！
dotenv.config();

// Discordボットを作成するためのクライアントを設定する！
// ここでは、Guilds（サーバー）とメッセージの内容を取得するための意図を指定
const client = new Client({
	intents: [
		GatewayIntentBits.Guilds,
		GatewayIntentBits.GuildMessages,
		GatewayIntentBits.MessageContent,
		GatewayIntentBits.GuildMembers,
	],
});

// ボットが準備できたときに実行されるイベントリスナーを設定する！
// ここでは、ボットがログインしたときにコンソールにメッセージを表示する
client.once('ready', () => {
	console.log(`${client.user.tag} としてログインしたよ！`);
});

// メッセージが送信されたときに実行されるイベントリスナーを設定する！
client.on('messageCreate', (message) => {
	if (message.author.bot) return;

	if (message.content === 'ping') {
		message.reply('pong!');
	}
});

// ボットをDiscordにログインさせるために、.envファイルからトークンを取得してログインする！
if (!process.env.DISCORD_TOKEN) {
	console.error('DISCORD_TOKENが設定されていません！');
	process.exit(1);
}

client.login(process.env.DISCORD_TOKEN);

// ここからは、Webサーバーをセットアップ！
// Expressを使って簡単なWebサーバーを作成する！
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Bot is awake!'));

app.listen(port, () =>
	console.log(`Webサーバーもポート ${port} で起きてるよ！`)
);
```

> このコードには二つの役割
>
> 1. `discord.js`を使って簡単な Bot を動かす(ping に pong!)
> 1. Render の仕組みに対応するための「Web サーバー」も`Express`で同時に動かす

## GitHub にリポジトリを作成

1. **GitHub**にログインし、新しいリポジトリを作成する
1. Repository name をわかりやすい名前に設定し、**Create repository**を押す
1. 真ん中の水色の枠の URL をコピーする

## GitHub にアップロードする

1. ターミナルに以下を順番に入力し、実行していく

```bash
git init
git add .
git commit -m "最初のBot設計図が完成！"
git branch -M main
git remote add origin #ここにGitHubのリポジトリURLを貼り付け
git push -u origin main
```

> この一連の手順で、ローカルのコードを GitHub のリポジトリにアップロードできる。<br>
> Render とも連携しやすくなる。

## Render で Bot を公開

1. Render にログイン
1. **+Add new** を押し、**Web Service**を選択
1. GitHub と連携し、先ほど作成したリポジトリを選択
1. 各項目を以下のように設定

| 設定項目      | 値              |
| ------------- | --------------- |
| Name          | 任意の名前      |
| Language      | Node            |
| Build Command | `npm install`   |
| Start Command | `node main.mjs` |
| Instance Type | Free            |

> **Start Command** は特に重要！

1. Environment Variables の**Add from .env**を押して、`.env`の中身をそのままコピペ
1. **Deploy Web Service**
1. ログを確認し、「サンプル bot#xxxx としてログインしたよ！」と出力されたのを確認

## Google Apps Script でスリープ対策

Render の無料プランでは、アクセスがないと Bot たスリープ状態に。<br>
**GAS** を使って定期的にアクセスを送ることで、Bot を起こし続けることができる。

### スクリプトの作成

1. Google Apps Script にアクセスし、「新しいプロジェクト」を作成
1. 以下のコードを貼り付け、自分の Render の URL に置き換える

```javascript
function keepBotAwake() {
	const url = '<https://あなたのRenderのURL.onrender.com/>'; // ← 自分のURLに変更！
	UrlFetchApp.fetch(url);
}
```

> UrlFetchApp()は GAS から特定の URL にアクセスする関数。<br>
> これを使って Render に定期アクセスし、Bot がスリープしないようにできる。

1. 一度手動で実行し、アクセス権を付与する。

> 「このアプリは Google で確認されていません」と警告が出る場合があるが、自分で作ったプライベートなスクリプトなので問題なし！<br>
> 「詳細を表示」→「(プロジェクト名)に移動」で続行できる！

### トリガーを設定(定期実行)

1. 左メニューの「トリガー」を開く
1. 右下の「トリガーを追加」ボタンをクリック
1. 以下のように設定

| 項目                       | 設定内容         |
| -------------------------- | ---------------- |
| 実行する関数               | `keepBotAwake`   |
| イベントのソース           | 時間主導型       |
| 時間ベースのトリガーの種類 | 分単位のタイマー |
| 実行間隔                   | 1 分おき         |

1. 保存する

## ping コマンドを実行してみよう

1. 先ほど Bot を入れた鯖で`ping`とメッセージを送ってみる
1. Bot から`pong!`と返ってくれば成功！
