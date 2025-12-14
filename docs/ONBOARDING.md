# htmx オンボーディングガイド

## 目次
1. [htmxとは？](#htmxとは)
2. [なぜhtmxを使うのか？](#なぜhtmxを使うのか)
3. [基本概念](#基本概念)
4. [サンプルアプリの使い方](#サンプルアプリの使い方)
5. [学習の進め方](#学習の進め方)

## htmxとは？

htmxは、HTMLの属性を使ってAJAXリクエストを簡単に実行できるJavaScriptライブラリです。
JavaScriptを書かずに、HTMLだけでモダンなインタラクティブWebアプリケーションを構築できます。

### 主な特徴
- **シンプル**: HTMLの属性を追加するだけで動的な機能を実装
- **軽量**: 約14kBの小さなライブラリ
- **サーバーサイド重視**: サーバーからHTMLを返すだけで動作
- **既存技術との相性**: どんなバックエンド言語とも組み合わせ可能

## なぜhtmxを使うのか？

### 1. JavaScriptの記述量を削減
複雑なJavaScriptフレームワークを学ぶ必要がありません。
HTMLの属性を追加するだけで、以下のような機能を実装できます：
- フォームの非同期送信
- ページの一部だけを更新
- リアルタイムな検索
- 無限スクロール

### 2. サーバーサイドの知識を活かせる
- 既存のサーバーサイドの知識をそのまま活用
- HTMLを返すだけなので、どんな言語・フレームワークでも使える
- ビジネスロジックをサーバー側に集約できる

### 3. メンテナンスしやすい
- HTMLとサーバーサイドコードだけを管理
- ビルドプロセスが不要または最小限
- 学習曲線が緩やか

## 基本概念

### コアアトリビュート（属性）

#### hx-get
指定したURLにGETリクエストを送信します。
```html
<!-- ボタンをクリックすると /api/data からデータを取得 -->
<button hx-get="/api/data">データを読み込む</button>
```

#### hx-post
指定したURLにPOSTリクエストを送信します。
```html
<!-- フォームを非同期で送信 -->
<form hx-post="/api/submit">
  <input name="message" type="text">
  <button type="submit">送信</button>
</form>
```

#### hx-target
レスポンスを表示する要素を指定します。
```html
<!-- レスポンスを#resultに表示 -->
<button hx-get="/api/data" hx-target="#result">データ取得</button>
<div id="result"></div>
```

#### hx-swap
レスポンスをどのように挿入するかを指定します。
- `innerHTML`: 要素の中身を置き換え（デフォルト）
- `outerHTML`: 要素自体を置き換え
- `beforebegin`: 要素の前に挿入
- `afterbegin`: 要素の最初の子として挿入
- `beforeend`: 要素の最後の子として挿入
- `afterend`: 要素の後に挿入

```html
<button hx-get="/api/data" hx-target="#result" hx-swap="beforeend">
  追加
</button>
<div id="result"></div>
```

#### hx-trigger
リクエストを発行するイベントを指定します。
```html
<!-- 入力時に自動で検索 -->
<input 
  type="text" 
  hx-get="/api/search" 
  hx-trigger="keyup changed delay:500ms"
  hx-target="#results">
```

## サンプルアプリの使い方

このリポジトリには、段階的に学べる複数のサンプルが含まれています。

### 必要なもの
- Webブラウザ（Chrome, Firefox, Safari, Edgeなど）
- シンプルなWebサーバー（Python、Node.js、またはその他）

### 起動方法

#### Pythonを使う場合
```bash
# Python 3の場合
cd samples/01-basic
python -m http.server 8000

# ブラウザで http://localhost:8000 を開く
```

#### Node.jsを使う場合
```bash
# http-serverをインストール（初回のみ）
npm install -g http-server

# サーバーを起動
cd samples/01-basic
http-server -p 8000

# ブラウザで http://localhost:8000 を開く
```

### サンプルの構成

#### 01-basic: 基本的な使い方
- `counter.html`: カウンターアプリ（hx-getの基本）
- `form.html`: フォーム送信（hx-postの基本）
- `search.html`: リアルタイム検索（hx-triggerの使い方）

#### 02-intermediate: 中級レベル
- `todo.html`: TODOアプリ（CRUD操作）
- `tabs.html`: タブ切り替え（hx-targetの活用）
- `infinite-scroll.html`: 無限スクロール（hx-triggerの応用）

#### 03-advanced: 応用例
- `validation.html`: フォームバリデーション
- `modal.html`: モーダルダイアログ
- `polling.html`: ポーリング（自動更新）

## 学習の進め方

### ステップ1: 基本サンプルを動かす
1. `samples/01-basic/counter.html` を開く
2. ブラウザの開発者ツールで Network タブを確認
3. ボタンをクリックして、どんなリクエストが送られているか観察
4. HTMLとモックサーバーのコードを読んで理解する

### ステップ2: コードを変更してみる
1. `hx-target` を変更して、表示先を変えてみる
2. `hx-swap` を変更して、挿入方法を変えてみる
3. `hx-trigger` を変更して、イベントを変えてみる

### ステップ3: 自分でサンプルを作る
1. 簡単なアプリを考える（例：いいねボタン、コメント機能）
2. 必要なHTMLを書く
3. htmx属性を追加する
4. モックサーバーを実装する

### ステップ4: 実際のサーバーと統合
1. 好きなバックエンド言語を選ぶ（Node.js、Python、Go、Rubyなど）
2. APIエンドポイントを実装する
3. HTMLを返すようにする
4. データベースと連携する

## よくある質問

### Q: JavaScriptは全く使わないの？
A: 基本的な機能はHTMLだけで実装できますが、カスタムな動作が必要な場合は
JavaScriptと組み合わせることもできます。htmxは既存のJavaScriptと共存できます。

### Q: どんなバックエンドが必要？
A: どんなバックエンドでも使えます。サーバーがHTMLを返せればOKです。
Node.js、Python（Flask/Django）、Ruby（Rails）、Go、PHPなど、
お好きな言語・フレームワークを選べます。

### Q: SEOへの影響は？
A: 初期レンダリングはサーバーサイドで行うため、SEOに優しいです。
検索エンジンのクローラーは初期HTMLを読み取れます。

### Q: 既存のプロジェクトに追加できる？
A: はい！htmxは段階的に導入できます。一部のページや機能から始めて、
徐々に拡大していくことが可能です。

## 次のステップ

1. **公式ドキュメント**: https://htmx.org/docs/
2. **サンプル集**: https://htmx.org/examples/
3. **htmxの哲学**: https://htmx.org/essays/

## 参考リソース

- [htmx公式サイト](https://htmx.org/)
- [htmx GitHub](https://github.com/bigskysoftware/htmx)
- [Hypermedia Systems（書籍）](https://hypermedia.systems/)

## トラブルシューティング

### レスポンスが表示されない
1. ブラウザの開発者ツールでNetworkタブを確認
2. リクエストが送信されているか確認
3. レスポンスのステータスコードを確認（200 OKか？）
4. レスポンスの内容を確認（HTMLが返ってきているか？）

### CORSエラーが出る
モックサーバーを使う場合は、同じオリジン（localhost:8000など）で
HTMLとAPIを提供してください。

### 動作が遅い
- `hx-trigger` で `delay` を設定して、リクエストを間引く
- サーバーのレスポンスを最適化する
- 必要に応じてキャッシュを活用する

---

このガイドを読んだら、実際にサンプルを動かして試してみましょう！
わからないことがあれば、コードを読んだり、ブラウザの開発者ツールで
動作を確認したりしながら、少しずつ理解を深めていってください。

Happy learning with htmx! 🚀
