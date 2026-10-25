# 1D4DD ウェブサイト（https://1d4dd.com）

作家・津久井五月の公式サイトのソースです。
**新しいチャットで Claude に修正を頼むときは、最初にこの README.md を読ませてください。**
サイトの構成、表記ルール、よくある修正の手順をここにまとめています。

---

## 1. 全体の仕組み

- ビルド不要の静的サイト（HTML と CSS だけ）。JavaScript は使っていません。
- ファイルを GitHub リポジトリに置き、**GitHub Pages** で公開しています。
  リポジトリのファイルを更新すると、1〜数分で https://1d4dd.com に反映されます。
- ドメイン（1d4dd.com）は **Squarespace** で取得・管理しています。
  DNS は Squarespace の管理画面で GitHub Pages を向いています（下の「8. 公開・ドメインの設定」）。
- メール（tsukui［at］1d4dd.com、Google Workspace）も同じドメインの DNS（MX レコードなど）で動いています。
  **DNS を触るときは MX レコードに手を付けないこと。**

## 2. ファイル構成

```
/
├── index.html          トップページ（プロフィール・新刊・主な作品・主なクライアントワーク）
├── works/index.html    全作品リスト（https://1d4dd.com/works/）
├── 404.html            存在しないURLを開いたときのページ
├── assets/
│   ├── style.css       全ページ共通のスタイル（色・余白・部品）
│   ├── favicon.svg     ブラウザのタブのアイコン
│   └── images/         画像の置き場所（profile.jpg＝プロフィール写真。書影・OGP画像もここへ）
├── CNAME               GitHub Pages 用の独自ドメイン設定（中身は 1d4dd.com）
├── .nojekyll           GitHub Pages の自動変換を止めるための空ファイル（消さない）
└── README.md           このファイル
```

ヘッダーとフッターは各HTMLに直接書いています（共通化の仕組みはなし）。
**ヘッダー・フッターを変えるときは index.html と works/index.html の両方を直す。**

## 3. デザインの基本（assets/style.css の :root）

| 用途 | 変数 | 値 |
|---|---|---|
| 背景 | `--bg` | `#454F55` |
| 基調色（ロゴ・見出し） | `--accent` | `#FF963A` |
| 本文 | `--text` | `#FFB577` |
| 補足（日付・媒体名・タグ） | `--sub` | `#E8C9AC` |
| 区切り線 | `--line` | `rgba(255,150,58,0.3)` |
| 差し色（リンク下線・矢印・ボタン枠） | `--link` | `#74CBCB` |
| 文章ブロックの最大幅 | `--measure` | `640px` |

- 書体は Noto Sans JP（Google Fonts）。
- 配色は暗い版のみ（明るい版は作らない方針）。
- スマホ（幅600px以下）ではヘッダーのナビゲーションとプロフィール写真を非表示にし、書影は画面幅いっぱいに表示。
- プロフィール写真（assets/images/profile.jpg、600×600px）はプロフィール文の右に160px四方で表示。写真の余白も大事なので、切り抜きや色の加工はしない。
- プロフィール文・新刊紹介・クライアントワークの説明文は、右端が揃うよう同じ最大幅（640px）。

## 4. 表記ルール

- **括弧**：書籍・雑誌・アンソロジーなど本の題名は『』、作品（短篇・記事・連載）は「」。
  例外：「ARTIFITECTS：模造建築家回顧録」は連載なので「」。
- **並び順**：どのリストも新しい順（上が最新）。
- **リンク**：オンラインで読める作品だけ、題名にリンクを付ける。
  題名に水色の下線、直後に水色の「↗」。リンクは新しいタブで開く。
  サイト内ページへの移動は「→」「←」。
- **形式のタグ**：小説・エッセイ・展示作品などの種類は、題名の後ろに小さな枠付きタグで示す。
- **区切り記号**：スラッシュは半角「/」。全角「／」は使わない。
- **ルビ**：全作品リストでは［］で残す（例：「友愛決定境界［フラターナル・ディシジョン・バウンダリ］」）。
  トップページの「主な作品」ではルビを付けない。
- **SFマガジン**：表記は「SFマガジン」に統一（「S-Fマガジン」とはしない）。
- **メールアドレス**：迷惑メール対策のため「tsukui［at］1d4dd.com」と表記し、リンクにしない。
- **日付**：トップページは「2025.06」、連載など期間があるものは「2023.04–」。
  全作品リストは年の見出しの下に「6月」。

## 5. よくある修正の手順

### 5-1. 作品を追加する

1. **全作品リスト**（works/index.html）の該当する年の `<ul class="rows rows--month">` に1行追加する。
   その年の見出しがなければ、`<section>` ごと新しく作って一番上（新しい年が上）に置く。
2. **トップページにも載せる代表作**なら、index.html の「主な作品」の該当グループ（長篇・中篇／短篇／ノンフィクション）に追加する。
3. **クライアントワーク**なら、index.html の「主なクライアントワーク」に追加し、全作品リストにも載せる。

行の書き方（コピーして使う）：

```html
<!-- リンクなし（全作品リスト） -->
<li class="row"><span class="row__date">5月</span><div class="row__body"><p>「作品名」</p><p class="row__meta">『掲載誌・書名』（出版社）</p></div></li>

<!-- リンクあり＋タグ（全作品リスト） -->
<li class="row"><span class="row__date">3月</span><div class="row__body"><p><a href="https://example.com/" target="_blank" rel="noopener"><span class="ul">「作品名」</span><span class="ar" aria-hidden="true">↗</span></a><span class="tag">エッセイ</span></p><p class="row__meta">ウェブメディア「〇〇」（運営会社）</p></div></li>

<!-- トップページ「主な作品」 -->
<li class="row"><span class="row__date">2025.06</span><div class="row__body"><p>「作品名」</p><p class="row__meta">SFマガジン 2025年8月号</p></div></li>

<!-- トップページ「主なクライアントワーク」（タグは形式、2行目はクライアント名） -->
<li class="row"><span class="row__date">2026.04</span><div class="row__body"><p><a href="https://example.com/" target="_blank" rel="noopener"><span class="ul">「作品名」</span><span class="ar" aria-hidden="true">↗</span></a><span class="tag">小説</span></p><p class="row__meta">クライアント名</p></div></li>
```

### 5-2. 新刊の書影を入れる

1. 画像を `assets/images/` に置く（例：`assets/images/cover.jpg`。横幅 600〜800px 程度、JPEG）。
2. index.html の新刊欄で、次の行を
   ```html
   <div class="newbook__cover is-placeholder">No Image</div>
   ```
   次のように置き換える（`is-placeholder` を外す）。
   ```html
   <div class="newbook__cover"><img src="/assets/images/cover.jpg" alt="『（書名）』書影" width="600" height="860"></div>
   ```
   書影は装丁家・出版社の著作物でもあるので、掲載前に担当編集者に確認する。

### 5-3. 新刊のボタンを使えるようにする

リンク先が決まったら、`<span class="btn is-disabled" aria-disabled="true">…</span>` を次の形に置き換える。

```html
<a class="btn" href="https://（出版社の本のページのURL）" target="_blank" rel="noopener">出版社のページ<span class="ar" aria-hidden="true">↗</span></a>
```

### 5-4. 新刊が刊行されたら

- 新刊欄の日付（`newbook__date`）・書誌（判型・ページ数・定価）・紹介文を確定版にする。
- 次の新刊が出たら、新刊欄を差し替え、前の本は「主な作品」と全作品リストに残す。
- プロフィール文の刊行に関する一文も見直す。

### 5-5. 新刊欄を非表示にする・表示に戻す

**現在（2026年10月時点）は新刊欄を非表示にしています。** 刊行情報を公開できるまでの措置です。
要素に `hidden` を付けると、中身を残したまま表示だけを消せます。次の3か所がセットです。

| ファイル | 該当箇所 |
|---|---|
| index.html | `<section class="section" id="novel" hidden>`（新刊欄そのもの） |
| index.html | ヘッダーのナビ `<a href="#novel" hidden>新刊</a>` |
| works/index.html | ヘッダーのナビ `<a href="/#novel" hidden>新刊</a>` |

**表示に戻すとき**は、この3か所の ` hidden` を消す。あわせて次の3つも元に戻す（非表示にした際に外したもの）。

- プロフィール文：「デビュー。」の後ろに「（年）、第二長篇『（書名）』を刊行。」を入れる
- index.html「主な作品」の長篇・中篇の先頭に
  `<li class="row"><span class="row__date">2027.01</span><div class="row__body"><p>『（書名）』</p><p class="row__meta">（出版社）</p></div></li>`
- works/index.html の一番上（2026年の前）に、2027年の見出しと
  `<li class="row"><span class="row__date">1月</span><div class="row__body"><p>『（書名）』</p><p class="row__meta">（出版社）</p></div></li>`

注意：`hidden` は表示を消すだけで、HTMLのソースを見れば中身は読める。まだ公にできない情報は、`hidden` に頼らず文言そのものを伏せること。

## 6. 未確定・今後の作業（TODO）

- [ ] 新刊欄の再表示（5-5）。公表前の情報（書名・出版社など）は、このリポジトリが公開されているため書かないこと。あわせて書影、判型・ページ数・定価、紹介文を確定版に（現在はダミー）
- [ ] 新刊ボタン2つのリンク先（現在は無効表示）
- [ ] OGP画像（SNSで共有されたときの画像。1200×630px）。用意したら `assets/images/ogp.png` に置き、
      index.html の `<head>` にあるコメントアウト部分を有効にする
- [ ] 必要に応じてファビコンを本格的なものに差し替え（assets/favicon.svg）

## 7. Claude に修正を頼むときの進め方

- GitHub リポジトリ（`tsukui-itsuki/1d4dd`）を Claude のチャットに追加（接続）して、「README.md を読んでから、〇〇を直して」と頼む。
  Claude が修正を `main` に保存（コミット・プッシュ）すると、GitHub Pages が自動で公開し直す。
- 文字の誤りなど小さな修正は、GitHub の画面でファイルを開き、鉛筆アイコンから直接編集してもよい。
- 変更の履歴は GitHub に残るので、問題があれば以前の状態に戻せる。
- 修正後は、PC幅とスマホ幅の両方で表示を確認する。

## 8. 公開・ドメインの設定（記録）

GitHub Pages（Settings → Pages）
- Source：Deploy from a branch ／ Branch：`main`、フォルダ：`/ (root)`
- Custom domain：`1d4dd.com`、Enforce HTTPS：オン

Squarespace の DNS レコード（ウェブサイト用）
| 種類 | ホスト | 値 |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | tsukui-itsuki.github.io |

ドメインの DNS は **Squarespace**（account.squarespace.com → ドメイン → 1d4dd.com → DNS設定）で管理。
メールは Google Workspace で、MX・SPF（TXT @）・DKIM（TXT google._domainkey）の3行がメール用。

※ MX レコード（メール用）は変更しない。
