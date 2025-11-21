# esports-autocaster
**For Tournament Organizers & Streamers.**

ゲームのスコア・ラウンド・キル・クラッチなどの状態を JSON で取得し、
Advanced Scene Switcher（ASS） と OBS を自動制御するシステムです。

「キルが起きたらリプレイ」「フリーズタイムはピクチャーインピクチャー」など、
これまでディレクターの勘と反射神経で行っていた演出を、ルールと AI で再現します。

## 概要
eSports AutoCaster は、「ゲームの中で起きていること」 と 「スイッチャーの操作」 を接続し、
大会配信のシーン切替・リプレイ・ハイライト演出を自動化するフレームワークです。

## Features
「ゲームの中で起きていること」と「スイッチャーの操作」を接続することで、 これまで人の勘と反射神経に頼っていた大会演出を、ルールとデータで再現します。

### 01. Game-Aware Scene Switching
**ゲームイベント連動のシーン自動切替**
ラウンド開始・フリーズタイム・ラウンド終了・クラッチ・スコア変化など、 ゲームから送られてくる JSON を Advanced Scene Switcher 用のイベントに変換。 InGame / FreezeTime / Result / Clutch などのシーンを自動で切り替えます。
対応例：
* Game State Integration / API / Overwolf などに対応
* ASS driver をゲームごとに設計可能
* AUTO / MANUAL 切り替えもホットキーで制御

### 02. Replay & Highlight
スコアが動いた瞬間やクラッチ発生時に、リプレイ用ホットキー（例: F10）を自動発火。 OBSのリプレイバッファや専用シーンと組み合わせることで、 「神プレイを逃さない」ハイライト演出が可能になります。
* スコア変化を検出してリプレイ実行
* クラッチ時は専用シーン（観客＋選手カメラ）に切替
* リプレイ再生後、元のシーンに自動復帰

### 03. Multi-Device Control
スマホからシンプル操作の UI も提供 PC の OBS / ASS を制御しつつ、スマホからは「Starting」「InGame」「Break」「End」 といった大きなボタンだけで操作できる Web UI を提供。 モバイルからトリガーホットキーを叩く構成も選べます。
* モバイルフレンドリーな 4 ボタン UI
* /api/obs/fkey 経由でホットキー発火
* 大会スタッフが手元の端末から操作可能

## Supported Titles
**対応予定ゲームタイトルとドライバー構成**
各ゲームごとに「Game 固有 JSON → 汎用 AssEvent」に変換するドライバーを用意。 一度フレームワークを作れば、新しいタイトルも比較的容易に追加できます。

### CS:GO
**CS:GO Game State Integration Driver**
gamestate_integration_*.cfg からローカル POST される JSON を受け取り、 ラウンドフェーズ・スコア・プレイヤー生存数・ボム状態などを解析。 ass-csgo.cjs で AssEvent に変換し、ASS / OBS を制御します。
* game.csgo.phase（menu / freezetime / live / over）
* game.csgo.score（T / CT スコア）
* game.csgo.bomb（planted / defused / none）
* game.csgo.playersAlive（クラッチ判定）

### VALORANT（構想）
**VALORANT API / Overwolf 連携**
ラウンド開始・スパイク設置 / 解除・クラッチ・エースなどのイベントを取得し、 CS:GO と同様のドライバー構造で AssEvent に変換する構想です。
* ラウンドフェーズごとのシーンレイアウト切替
* スパイク設置時の特別演出トリガー
* エース時のカメラ＋テロップ制御

### その他タイトル
**カスタムドライバーで拡張可能**
API / ログ / OCR など、ゲームによって取れる情報はさまざまですが、 「Game JSON → AssEvent → Hotkey」というパターンさえ守れば、 どのタイトルでも同じフレームワークで自動化が可能です。
* LoL / Apex / PUBG / etc...
* カスタムトーナメントツールとの連携も可能
* OBS 以外のスイッチャーにも応用しやすい構造

## Architecture
**eSports AutoCaster のイベント駆動アーキテクチャ**
「Event → AssEvent → Hotkey / Scene」というシンプルな三層構造がコア。
AI に全てを任せるのではなく、ディレクターが理解しやすいルールベースを中心 に設計されています。

## Event Layer
ゲームクライアントや API から送られる「ゲーム固有 JSON」を受信。
例：
* CS:GO GSI JSON
* VALORANT ラウンドイベント
* トーナメントツールの Webhook

## AssEvent Layer
ass-game.cjs ドライバーが Game JSON を汎用イベントへマッピング。
役割：
* 状態を ASS が扱いやすい共通形式へ変換
* シーン切替 / リプレイ用トリガー生成
* 現在シーン / ロック状態の管理

## Output Layer
AssEvent に応じて、OBS / ASS へ制御信号を送出。
内容：
* ホットキー送信
* シーン切替
* マクロ実行
* 必要に応じて AI による「盛り上がりスコア」やテロップ生成

## License
使用するライセンスを記述
