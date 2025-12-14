# htmx クイックリファレンス

htmxでよく使う属性のクイックリファレンスガイドです。

## 📋 基本的な属性

### リクエスト送信

| 属性 | 説明 | 例 |
|------|------|-----|
| `hx-get` | GETリクエストを送信 | `<button hx-get="/api/data">取得</button>` |
| `hx-post` | POSTリクエストを送信 | `<form hx-post="/api/submit">` |
| `hx-put` | PUTリクエストを送信（更新） | `<button hx-put="/api/update">更新</button>` |
| `hx-delete` | DELETEリクエストを送信 | `<button hx-delete="/api/delete">削除</button>` |
| `hx-patch` | PATCHリクエストを送信 | `<button hx-patch="/api/patch">修正</button>` |

### レスポンス制御

| 属性 | 説明 | 例 |
|------|------|-----|
| `hx-target` | レスポンスを表示する要素 | `hx-target="#result"` |
| `hx-swap` | レスポンスの挿入方法 | `hx-swap="innerHTML"` |
| `hx-include` | 追加で送信する要素 | `hx-include="#other-input"` |
| `hx-select` | レスポンスから特定の部分だけ選択 | `hx-select="#content"` |

### hx-swap の値

| 値 | 説明 |
|-----|------|
| `innerHTML` | 要素の中身を置き換え（デフォルト） |
| `outerHTML` | 要素自体を置き換え |
| `beforebegin` | 要素の前に挿入 |
| `afterbegin` | 要素の最初の子として挿入 |
| `beforeend` | 要素の最後の子として挿入 |
| `afterend` | 要素の後に挿入 |
| `delete` | 要素を削除 |
| `none` | レスポンスを表示しない |

### トリガー制御

| 属性 | 説明 | 例 |
|------|------|-----|
| `hx-trigger` | リクエストを発行するイベント | `hx-trigger="click"` |

### hx-trigger の主な値

| 値 | 説明 |
|-----|------|
| `click` | クリック時（ボタンのデフォルト） |
| `change` | 変更時（inputのデフォルト） |
| `submit` | 送信時（formのデフォルト） |
| `keyup` | キーを離したとき |
| `keydown` | キーを押したとき |
| `load` | 要素が読み込まれたとき |
| `revealed` | 要素が画面に表示されたとき |
| `every Xs` | X秒ごとに定期実行 |

### hx-trigger の修飾子

| 修飾子 | 説明 | 例 |
|--------|------|-----|
| `once` | 一度だけ実行 | `hx-trigger="click once"` |
| `changed` | 値が変更されたときだけ | `hx-trigger="keyup changed"` |
| `delay:Xms` | X ms待ってから実行 | `hx-trigger="keyup delay:500ms"` |
| `throttle:Xms` | X ms以内に1回だけ実行 | `hx-trigger="scroll throttle:1s"` |
| `from:セレクタ` | 別の要素からのイベント | `hx-trigger="click from:body"` |

## 🎨 UI・UX関連

| 属性 | 説明 | 例 |
|------|------|-----|
| `hx-indicator` | ローディング表示の要素 | `hx-indicator="#spinner"` |
| `hx-confirm` | 実行前に確認ダイアログ | `hx-confirm="削除しますか？"` |
| `hx-prompt` | 入力プロンプトを表示 | `hx-prompt="名前を入力"` |
| `hx-disable` | リクエスト中は無効化 | `hx-disable` |

## 🔄 ナビゲーション・履歴

| 属性 | 説明 | 例 |
|------|------|-----|
| `hx-push-url` | ブラウザ履歴に追加 | `hx-push-url="true"` |
| `hx-replace-url` | 現在のURLを置き換え | `hx-replace-url="true"` |
| `hx-boost` | リンクやフォームをAJAX化 | `hx-boost="true"` |

## 🎯 実践的なパターン

### 1. リアルタイム検索
```html
<input 
  type="text" 
  name="q"
  hx-get="/search"
  hx-trigger="keyup changed delay:500ms"
  hx-target="#results">
<div id="results"></div>
```

### 2. 無限スクロール
```html
<div 
  hx-get="/api/next-page"
  hx-trigger="revealed"
  hx-swap="afterend">
  トリガー要素
</div>
```

### 3. 自動更新（ポーリング）
```html
<div 
  hx-get="/api/status"
  hx-trigger="every 2s"
  hx-swap="innerHTML">
  ステータス表示
</div>
```

### 4. フォーム送信後のリセット
```html
<form 
  hx-post="/api/submit"
  hx-on::after-request="this.reset()">
  <input name="data">
  <button type="submit">送信</button>
</form>
```

### 5. 削除確認
```html
<button 
  hx-delete="/api/item/123"
  hx-confirm="本当に削除しますか？"
  hx-target="#item-123"
  hx-swap="outerHTML">
  削除
</button>
```

### 6. ローディング表示
```html
<button 
  hx-get="/api/data"
  hx-indicator="#spinner">
  データ取得
</button>
<div id="spinner" class="htmx-indicator">
  読み込み中...
</div>
```

CSS:
```css
.htmx-indicator {
  display: none;
}
.htmx-request .htmx-indicator {
  display: block;
}
```

## 🎭 CSSクラス

htmxが自動的に追加するCSSクラス：

| クラス | タイミング | 用途 |
|--------|-----------|------|
| `htmx-request` | リクエスト中 | ローディング表示 |
| `htmx-swapping` | スワップ中 | アニメーション |
| `htmx-settling` | セトリング中 | アニメーション |
| `htmx-added` | 要素追加時 | フェードイン |

## 📚 より詳しく

- [htmx公式ドキュメント](https://htmx.org/docs/)
- [htmxサンプル集](https://htmx.org/examples/)
- [このリポジトリのオンボーディングガイド](./ONBOARDING.md)

## 💡 Tips

1. **デバッグ**: ブラウザの開発者ツールのNetworkタブでリクエストを確認
2. **エラー処理**: `hx-on::response-error` でエラーハンドリング
3. **アニメーション**: CSSトランジションと組み合わせて滑らかな動き
4. **パフォーマンス**: `delay` や `throttle` で不要なリクエストを削減
5. **アクセシビリティ**: ARIA属性も忘れずに追加

---

このリファレンスがhtmx開発の助けになれば幸いです！
