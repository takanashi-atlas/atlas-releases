# ATLAS

声だけで使うAI秘書「ATLAS(アトラス)」の配布ページです。
提供:たかなし(X:[@takanashi_ai120](https://x.com/takanashi_ai120))

使うには、提供者から届く **利用キー** が必要です(キーのない人がダウンロードしても使えません)。

## ダウンロード

最新版は [Releases](https://github.com/takanashi-atlas/atlas-releases/releases/latest) にあります。

| パソコン | ファイル |
|---|---|
| Windows 10 / 11 | `ATLAS-Setup-<版>.exe` |
| Mac(Appleのチップ:M1〜) | `ATLAS-Mac-<版>-AppleChip.zip` |
| Mac(Intel) | `ATLAS-Mac-<版>-Intel.zip` |

必要なもの:Google Chrome、マイク、インターネット。Macは macOS 13.5 以降。

---

## 詳しい方へ:ATLASがすること・しないこと

ATLASは、あなたのパソコンの中で動く小さなウェブアプリです。
画面はChromeで開き、AIへの問い合わせだけがインターネットに出ます。

### パソコンに入れるもの

**Windows**(管理者の権限は使いません。今のユーザーだけに入ります)

| 場所・名前 | 中身 |
|---|---|
| `%LOCALAPPDATA%\ATLAS` | プログラム一式(Node.js 24 の公式版を同梱) |
| `%LOCALAPPDATA%\ATLAS\data` | 設定・覚えたこと・お知らせ・利用キー |
| `%LOCALAPPDATA%\ATLAS\logs` | 動作の記録 |
| タスク「ATLAS App Watchdog」 | 止まっていたら起動し直す見張り役(「起動時に準備する」を選んだときだけ) |
| スタートアップの「ATLAS」 | ログイン時に起動(同上) |

**Mac**(管理者の権限、カーネル拡張、フルディスクアクセスは使いません)

| 場所・名前 | 中身 |
|---|---|
| `/Applications/ATLAS.app`、`ATLASを削除.app` | 起動用と削除用(中の起動プログラムは、本体のスクリプトを動かすだけの約30行のC言語) |
| `~/Library/Application Support/ATLAS/runtime` | プログラム一式(Node.js 24 の公式版を同梱) |
| `~/Library/Application Support/ATLAS/data` | 設定・覚えたこと・お知らせ・利用キー |
| `~/Library/LaunchAgents/com.takanashi.atlas.plist` | ログイン中に動かし続ける設定 |
| `~/Library/LaunchAgents/com.takanashi.atlas.open.plist` | ログイン時に画面を開く設定 |
| `~/Library/Logs/ATLAS` | 動作の記録 |

### 待ち受け(パソコンの中だけ)

- `127.0.0.1:3900`(画面)と `127.0.0.1:4274`(会話の処理)だけで待ち受けます。
- ほかのパソコンやスマホからはつながりません。
- 同じパソコンの中でも、ATLASの画面以外からの呼び出しは断ります(送り元の確認あり)。

### 通信する相手

| 相手 | 何のために | 送るもの |
|---|---|---|
| `ai-gateway.vercel.sh`(Vercel AI Gateway) | 返事を作る(AIはAnthropicのClaude)、ネット検索(Exa) | 話した内容、会話の流れ、答えに必要な範囲のメモ・書類の一部 |
| Google(Chromeの音声認識) | 声を文字にする | 話した声。ATLASではなく、Chromeの機能です |
| `github.com`(このページ) | 新しい版の確認と取得(Windowsのみ、6時間ごと) | なし(ダウンロードするだけ) |

上の3つ以外への通信はしません。広告や、使い方を集めて送る仕組み(テレメトリ)も入っていません。

### 使う権限・使わない権限

- **マイク**:Chromeが声を聞き取るために使います。
  - Windowsで「アトラス起動」と呼んで開く機能は、Windowsに入っている音声認識をパソコンの中だけで使います(音声は外に出ません)。
- **ファイル**:設定で選んだフォルダの中だけを読みます。
  - パスワードなどの名前のファイルは読みません。
  - 書き込むのは、メモのフォルダの中の「アトラス受信箱」(「メモして」と頼んだとき)と、ATLAS自身のデータの場所だけです。
- **使わないもの**:コマンドの実行、ほかのアプリの操作、メールの送信、画面の撮影。
- **書類の読み取り**:PDF・Word・Excelを文字にする作業は、メモリと時間に上限をつけた別の小さなプログラムで行います。中身がおかしいファイルでも本体は止まりません。

### 自動アップデート(Windows)

- 6時間ごとに、このページの `latest.json` を確認します。
- `latest.json` には提供者の鍵で **Ed25519署名** がしてあります。ATLASは、中に入っている公開鍵で確かめられたものだけを信じます。
- ダウンロードしたインストーラーは、`latest.json` に書かれた大きさと **SHA-256** が一致しないと捨てます。
- 入れ替えは、パソコンの起動時か夜3〜5時だけです。失敗したら元の版で動き続けます。

### ファイルが本物か確かめる

ダウンロードしたファイルのSHA-256(指紋)を、次の表と比べてください。

- Windows:`certutil -hashfile <ファイル> SHA256`
- Mac:`shasum -a 256 <ファイル>`

**版 0.1.9**

| ファイル | SHA-256 |
|---|---|
| `ATLAS-Setup-0.1.9.exe` | `0df839fbbce5f86bc9be52a68bb9deb52eddaccbade32760067034b4e759a60e` |
| `ATLAS-Mac-0.1.9-AppleChip.zip` | `a8a629a4f1a7f072305c55664a1835065d1005654f22c9b4cd48d43934a04727` |
| `ATLAS-Mac-0.1.9-Intel.zip` | `cfc034ad15ba71c1fca2c9d81a1eff94e06ad45435ed415e0270256753ed7624` |
| `ATLAS-Mac-0.1.9.zip`(両方のチップ用) | `316c12cfef99e7ce30182e5a949977eb0d3fec9dbe0a73e40edc3128499007f6` |

### ウイルス検査の結果(VirusTotal)

70ほどのウイルス対策ソフトでまとめて検査できるサイトの結果です(検査日:2026-10-25)。

| ファイル | 結果 |
|---|---|
| `ATLAS-Setup-0.1.9.exe` | [危険と判定したソフト:なし](https://www.virustotal.com/gui/file/0df839fbbce5f86bc9be52a68bb9deb52eddaccbade32760067034b4e759a60e) |
| `ATLAS-Mac-0.1.9-AppleChip.zip` | [結果を見る](https://www.virustotal.com/gui/file/a8a629a4f1a7f072305c55664a1835065d1005654f22c9b4cd48d43934a04727)(検査待ち) |
| `ATLAS-Mac-0.1.9-Intel.zip` | [危険と判定したソフト:なし](https://www.virustotal.com/gui/file/cfc034ad15ba71c1fca2c9d81a1eff94e06ad45435ed415e0270256753ed7624) |

ご自分で検査することもできます:[VirusTotal](https://www.virustotal.com/) にダウンロードしたファイルをそのまま渡すと、同じ結果のページが出ます。

### 署名について(正直に)

- **Windows**:コード署名の証明書はまだ使っていません。初回に「WindowsによってPCが保護されました」と出ます。
- **Mac**:Appleの開発者署名・公証はまだです(Appleの決まりに沿った「名前なし署名」のみ)。初回は「システム設定 → プライバシーとセキュリティ → このまま開く」が必要です。

### 消し方

- **Windows**:「設定 → アプリ → インストールされているアプリ → ATLAS → アンインストール」。データも消すかを聞かれます。
- **Mac**:「ATLASを削除」を開く。プログラム、ログイン時の設定、記録を消し、データも消すかを聞きます。最後に2つのアプリをゴミ箱へ入れてください。

あなたのメモや書類のフォルダは、どちらの場合も消えません。
