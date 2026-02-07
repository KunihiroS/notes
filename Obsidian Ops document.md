---
share: true
---
# はじめに
本ドキュメントは、ObsidianをUbuntu環境上で構築運用することための参照ドキュメントです。
![](Obsidian%20Ops%20document/Gemini_Generated_Image_poiz21poiz21poiz.png)
# 前提情報
- OS
	- Ubuntu 24.04
- Install
	- .deb
		- Snap versionはElectronとUbuntu GUI Waylandの相性問題で日本語入力ができないので利用しない。
- 利用ユーザ数
	- 1
- 課金の有無
	- なし
- データ同期
	- ローカルのVaultをGithubでレポ管理。
	- 作業前のプル及び完了後コミットが必須。
- 別リポジトリの取り込み（シンボリックリンク方式）
	- 他のGitリポジトリをObsidianから参照したい場合、シンボリックリンクで取り込むことが可能。
	- Obsidianはシンボリックリンク先のフォルダを通常のフォルダとして認識し、対応ファイル形式（Markdown、画像等）を開ける。
	- **前提条件**
		- Vaultリポジトリとターゲットリポジトリが同じ親ディレクトリ（兄弟関係）にあること。
		- 例: `~/{hpge}`（Vault）と `~/{foo}`（別リポ）
	- **手順（相対パスでの作成）**
		1. Vaultディレクトリに移動
			```bash
			cd /path/to/your/vault
			```
		2. 相対パスでシンボリックリンクを作成
			```bash
			ln -s ../target-repo target-repo
			```
		3. Gitにコミット（ポータブルにする場合）
			```bash
			git add target-repo
			git commit -m "Add symlink to target-repo"
			```
	- **Git管理の選択肢**
		- **リンクをコミットする（推奨）**: 相対パスで作成すれば、兄弟ディレクトリ構成を維持する限り別マシンでもclone後すぐ使える。
		- **リンクをコミットしない**: `.gitignore`に追加し、各マシンで手動作成。パス構成が異なる環境向け。
	- **注意事項**
		- 別リポ側のGit操作（pull/push）は独立して行う必要あり。
		- Obsidian Syncなど他の同期サービス併用時は二重管理に注意。
		- 絶対パスでリンクを作成するとパスが一致しない環境でリンク切れになる。
- その他
	- notionからの引っ越し先として選定。
# 利用ユース想定
- 個人の知識ベースとして利用。
- 個人のブレインストーミングや新しい発想の場、執筆などに利用を想定。
- AIと連携し個人の知識ベースとして知識活動のサイクルに組み込む。
- 論文を読んだ記録の保管庫としても活用。
# 制限事項
- ポストの公開は無課金で不可のため、公開のみnotionの利用を検討。
# Plug-in
- 利用コミュニティプラグイン
	- [Github sync](https://github.com/kevinmkchin/Obsidian-GitHub-Sync)
		- ワンクリックでGithubにコミット
		- VSCなどで管理をしたほうが事故がすくない
		- このプラグイン特有のトラブルがないとはいえない
		- 必須度: B
	- [obsidian-custom-attachment-location](https://github.com/mnaoumov/obsidian-custom-attachment-location)
		- 指定の名称でノート内の添付ファイルをディレクトリに保存する。
		- 例えば、{hoge}.md のノート内の添付ファイル {foo}.jpg が自動で ./{hoge}/{foo}.jpg として保存される。
		- ファイル変換やリネーム等機能多数あり。
		- 必須度: A
	- [obsidian-colored-text](https://github.com/erincayaz/obsidian-colored-text)
		- 選択範囲に対して右クリックコンテキストメニューでフォントカラーを変更できる。
		- ただし、単にタグを挿入する機能なので既存で色付けした範囲にさらに色を上書きしようとするとタグが二重になる。
		- 必須度: C
	- [Image converter](https://github.com/xRyul/obsidian-image-converter)
		- 画像を右クリックコンテキストメニューから変換
		- 必須度: B
	- [GitHub - SebastianMC/obsidian-custom-sort: Take full control over the order and sorting of folders and notes in File Explorer in Obsidian](https://github.com/SebastianMC/obsidian-custom-sort)
		- sortspecのルールファイルに沿ってファイルやディレクトリを区別なくソート
		- ノートとディレクトリを同じ名前で管理している場合、並びになるので参照しやすい
		- 必須度: B
	- [GitHub - wenlzhang/obsidian-folder-navigator: An Obsidian plugin that allows you to quickly navigate to folders in your vault using fuzzy search.](https://github.com/wenlzhang/obsidian-folder-navigator)
		- フォルダー検索
		- デフォルトの検索はフォルダを名称で検索できないので、フォルダーを探すために利用
		- リボンではなくコマンドパレットから呼び出し
		- 必須度: B
	- [GitHub - scambier/obsidian-omnisearch: A search engine that "just works" for Obsidian. Supports OCR and PDF indexing.](https://github.com/scambier/obsidian-omnisearch)
		- 高機能検索
		- 必須度: B
	- [GitHub - PKM-er/Obsidian-Surfing: An Obsidian plugin that lets you browse the web within Obsidian.](https://github.com/PKM-er/Obsidian-Surfing)
		- 組み込みWeb viewer
		- 必須度: B
	- [GitHub - brianpetro/obsidian-smart-connections: Chat with your notes & see links to related content with AI embeddings. Use local models or 100+ via APIs like Claude, Gemini, ChatGPT & Llama 3](https://github.com/brianpetro/obsidian-smart-connections)
		- ベクトル埋め込みを実施しノートに対して様々な処理を実施できる高機能プラグイン
		- 例えば、類似度によるノート一覧やセマンティックサーチなど
		- 高機能故に重いのでOmniSearchがあれば不要かもしれない
		- 必須度: B
	- [GitHub - logancyang/obsidian-copilot: THE Copilot in Obsidian](https://github.com/logancyang/obsidian-copilot)
		- ~~Vaultをグラウンドデータとしてインタラクションが可能~~
		- ~~ただし基本的にはKeyを用意したほうがいい~~
		- ~~これがあればSmart Connectionは不要かもしれない~~
		- 必須度: E
		- 動作が遅いし埋め込みをしてないからか精度が悪い
	- [GitHub - zolrath/obsidian-auto-link-title: Automatically fetch the titles of pasted links](https://github.com/zolrath/obsidian-auto-link-title)
		- URL貼り付けで情報を取得し適切なタイトルに自動変換
		- 必須度: B
	- [GitHub - Enveloppe/obsidian-enveloppe: Enveloppe helps you to publish your notes on a GitHub repository from your Obsidian Vault, for free!](https://github.com/Enveloppe/obsidian-enveloppe)
		- 指定したGithub repoにノートをアップロードできる
		- 簡易的に公開するために使える
		- frontmatter (mdファイルの先頭) に指定のプロパティを記載する必要があり
		- 必須度: B
	- [GitHub - glowingjade/obsidian-smart-composer: AI chat assistant for Obsidian with contextual awareness, smart writing assistance, and one-click edits.](https://github.com/glowingjade/obsidian-smart-composer)
		- Cursor AIやChatGPT Canvasのように、Vault内の文脈を理解したAIアシスタント。
		- 「Apply Edit」機能により、AIの提案をワンクリックでノートに反映可能。
		- セマンティック検索（RAG）により、関連するノートを自動で参照。
		- Googleなどのサブスクリプションベースの認証情報を使うことでAPI Key不要で使えるがCLIを動かしていると思われ、レスポンスが速くない
		- 必須度: B
	- [GitHub - gitcpy/obsidian-diagram-popup](https://github.com/gitcpy/obsidian-diagram-popup)
		- Mermaidなどダイアグラムのプレビューをワンクリックで拡大でき、更に拡大や縮小など可能。
		- 必須度: A
	- [GitHub - zsviczian/obsidian-excalidraw-plugin: A plugin to edit and view Excalidraw drawings in Obsidian](https://github.com/zsviczian/obsidian-excalidraw-plugin?tab=readme-ov-file)
		- 図を作成できる
		- 個人的にはそんなに使いどころはないけど便利は便利
		- 必須度: B
	- [GitHub - polyipseity/obsidian-terminal: Integrate consoles, shells, and terminals.](https://github.com/polyipseity/obsidian-terminal)
		- Terminalが使えるのは便利な場合もあるがスキル次第
		- 必須度: C
	- [GitHub - HEmile/obsidian-search-on-internet: Add context menu items in Obsidian to search the internet.](https://github.com/HEmile/obsidian-search-on-internet)
		- ノート名や選択テキストからGoogleもしくはWikipedia検索
		- 必須度: A
	- [GitHub - KunihiroS/local-referer: Obsidian plugin extends context menu available refer local files to insert.](https://github.com/KunihiroS/local-referer)
		- 右クリックからローカルファイルを添付できる
		- 必須度: A
	- [GitHub - KunihiroS/note-deep-researcher: Obsidian plugin creates Deep Research based on a note by Gemini API.](https://github.com/KunihiroS/note-deep-researcher)
		- ノートをコンテキストにGemini Deep Researchを実行してレポートを保存する
		- 便利だけどAPI利用費用が高い
		- 必須度: C
	- [GitHub - KunihiroS/genimage-inserter: Obsidian plugin makes available insert gened image as selected context.](https://github.com/KunihiroS/genimage-inserter)
		- ノート全体もしくは選択範囲のテキストをコンテキストにGemini画像生成しノート内に挿入する
		- Gemini3proは有用性が高い
		- 必須度: B
# Obsidian Community Plugin 初回リリース手順（簡易メモ）

## 0. 前提
- 公開用GitHubリポジトリがある
- manifest.json / versions.json / main.js が揃っている（ビルド済み）
### 0.1 Botで落ちやすい罠（先に知っておく）
- PR本文（テンプレ）に貼るとき、IDEや一部のUIが勝手にリンク変換して `cci:` や `<https://...>` になることがある
    - **テンプレ判定が壊れる原因**になるので、PR本文は **GitHub上で直接編集**するのが安全
    - README.md 等は **リンクにせずバッククォート（README.md）**推奨
- `description` は **PR（community-plugins.json）と repo（manifest.json）と最新Releaseのmanifest.json で完全一致**が必要
    - 大文字小文字・ピリオド含め一致（Review Botが機械的に比較）
- `description` に **`Obsidian` を入れると落ちる**（チェックで弾かれる）
## 1. リリース前チェック（最低限）
- manifest.json
    - `id`：最終決定（公開後は基本変えない）
    - `version`：これから出すバージョン
    - `minAppVersion`：最低対応Obsidian
    - `description`：
        - 末尾は `.` `?` `!` `)` のいずれかで終わる
        - **`Obsidian` を含めない**
- versions.json
    - `"manifest.version": "manifest.minAppVersion"` の対応があること
- `npm run build` が通ること
    
## 2. GitHub Release を作成（Obsidianが取得する配布物）
- GitHub → Releases → Draft a new release
- Tag / Release name：
    manifest.json の `version` と完全一致
    - 例：`1.0.0`（`v1.0.0` は使わない）
- Release assets（個別添付が必須）
    - main.js
    - manifest.json
    - styles.css（ある場合）
- 注意：source code zip/tar だけではNG（assetsとして個別ファイルを添付する）

### 2.1 Releaseに関する追加注意
- Review Botは「repoのmanifest」と「最新Releaseのmanifest」を参照して整合性を見ることがある
    - なので 
        manifest.json / versions.json は **コミット＆push**してからReleaseを作るのが安全
        
## 3. Community catalog へ登録PR（初回のみ）
- `obsidianmd/obsidian-releases` をFork
- 新しいブランチを切る（既存PRと混ぜない）
- community-plugins.json にプラグイン情報を追加
    - manifest.json の `id` と一致させる
    - 新規エントリは末尾に追加（チェックで落ちやすいポイント）
    - `repo` は公開リポジトリURLを指定
    - `description` は **manifest.json の description と完全一致**させる（重要）
- PRを作成し、テンプレのチェックリストに回答
- Botチェック → 人手レビュー待ち

### 3.1 PR本文（テンプレ）の注意（超重要）
- PR本文は「テンプレ文言が含まれているか」を機械的に判定している
    - 文言がリンク化（`<https://...>`）されたり、README.md がリンクになったりすると **テンプレ不一致で落ちる**ことがある
- 対策：
    - PR本文は **GitHub上のPR画面で直接Edit**する    
    - README.md は **README.md（バッククォート）**で書く（リンクにしない）
    - 一度通ったら、PR本文は極力触らない（再度落ちるリスク）

## 4. 公開後の運用（更新リリース）
- 修正が必要なら `version` を上げて新Release（例：`1.0.1`）
- 既存の `1.0.0` を差し替えない（混乱の元）
    
## 更新時にやること（いつもの手順）
Community pluginsに掲載済みのプラグインを更新するとき
- **manifest.json の `version` を上げる** 
- **versions.json にそのversion行を追加**
- `npm run build`
- **GitHub Release作成**
    - Tag/Release名 = manifest.json の `version`（`v`無し）
    - assets: main.js / manifest.json / styles.css（ある場合）

### 更新時の追加注意（Bot対策）
- `description` を変えた場合は必ず
    - repoの manifest.json
    - 最新Release assets の manifest.json
    - （必要なら）
        community-plugins.json の `description` を **完全一致**に揃える

# 例外：PRが必要になるケース（掲載済みでも）
次のどれかに当たると、`obsidian-releases` 側の更新PRが必要になることがあります。
- **リポジトリURLを変更**した（移転・リネーム等）
- **メンテナー移管**などでカタログ側の情報更新が必要
- **plugin id を変えたい**（基本NG。やるなら別プラグイン扱いになり得る）
- カタログのエントリ自体に誤りがあって修正したい