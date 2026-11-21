# uiux-review チェックリスト

6つの観点ごとに、コードで確かめる項目と根拠をまとめる。数値は各資料に書かれている基準（2026-11-21 時点）。

## 1. 認知（理解・記憶の限界）

根拠: [NN/g 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)、[Laws of UX](https://lawsofux.com/)

- 状態が見えるか: 読み込み中・空・エラー・成功の4状態が、それぞれ画面に出るか（ヒューリスティック1: システム状態の可視化）
- ユーザーの言葉か: エラーコード、例外メッセージ、内部の項目名をそのまま表示していないか（ヒューリスティック2）
- 取り消せるか: 削除など元に戻せない操作に、確認か取り消し（undo）があるか（ヒューリスティック3）
- 一貫しているか: 同じ操作に同じ言葉・位置・見た目を使っているか（ヒューリスティック4）
- 間違いを防ぐか: 押し間違えやすい位置に破壊的な操作を置いていないか（ヒューリスティック5）
- 覚えなくてよいか: placeholder をラベルの代わりにしていないか（入力を始めると消えて、何の欄か分からなくなる）（ヒューリスティック6）
- エラーから立ち直れるか: エラー文が「何が起きたか」と「どうすればよいか」を言っているか（ヒューリスティック9）

## 2. 時間（待てる長さ）

根拠: [NN/g Response Times](https://www.nngroup.com/articles/response-times-3-important-limits/)（0.1秒 / 1秒 / 10秒）、[web.dev RAIL](https://web.dev/articles/rail)、[INP](https://web.dev/articles/inp)（200ms 以下が良好）

- 押した瞬間に反応するか: ボタンを押したら、通信の完了を待たずに見た目が変わるか（押下状態、送信中の表示）
- 1秒を超えうる処理にインジケータがあるか: TanStack Query なら `isPending` / `isFetching` を表示に使っているか
- 10秒を超えうる処理に、進み具合と中断手段があるか
- 二重送信を防いでいるか: 送信中に同じ操作を繰り返せないか（`isPending` 中の扱い）
- 再取得中に既存の表示を消していないか: バックグラウンドの再取得で、一覧が一瞬空になったり、ちらついたりしないか
- レイアウトがずれないか: 読み込み後に要素が差し込まれて、押そうとした場所がずれないか（領域の大きさを確保しているか）
- 楽観的更新（先に画面を変えて、後で通信する方法）が失敗したとき、元に戻して失敗を伝えているか
- バックエンドとのつながり: API の応答時間、タイムアウト、エラーの返し方が、そのまま待ち時間の体験になる

## 3. 操作（指・マウス・キーボード）

根拠: [WAI-ARIA APG](https://www.w3.org/WAI/ARIA/apg/)、[WCAG 2.5.8 Target Size](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html)（最低 24×24 CSS px）、Apple HIG（44pt 推奨）

- 押せる範囲が十分か: ボタン・リンク・チェックボックスが 24×24px 以上か。小さいアイコンだけのボタンに注意
- 正しい要素か: 押せるものが `<button>` / `<a href>` か。`<div onClick>` はキーボードで押せず、読み上げでもボタンと伝わらない
- キーボードだけで全操作できるか: Tab で移動、Enter / Space で実行、Esc で閉じる
- フォーカスが見えるか: `outline: none` を代わりの表示なしで使っていないか（`:focus-visible` を使う）
- フォーカスの行き先が正しいか: モーダルを開いたら中へ移り、閉じたら元の場所へ戻るか。削除後にフォーカスが行方不明にならないか
- 複雑な部品（タブ、メニュー、コンボボックスなど）が APG のパターンどおりのキー操作になっているか

## 4. 入力（フォーム）

根拠: [GOV.UK Design System](https://design-system.service.gov.uk/)（[Error message](https://design-system.service.gov.uk/components/error-message/) / [Error summary](https://design-system.service.gov.uk/components/error-summary/) / [Validation](https://design-system.service.gov.uk/patterns/validation/)）、[web.dev Learn Forms](https://web.dev/learn/forms)

- ラベルがあるか: すべての入力欄に `<label for>`（または `aria-labelledby`）が紐づいているか
- 入力の手間を減らしているか: 適切な `type`（email、tel、number など）、`inputmode`、`autocomplete` を使っているか
- エラーが欄の近くに出るか: エラー文が該当する欄のそばにあり、`aria-describedby` で欄に紐づき、`aria-invalid` が付くか
- エラー文が直し方を言っているか: 「不正な値です」ではなく「タイトルを入力してください」のように具体的か
- エラー時に入力を消していないか: 送信に失敗しても、入力した内容が残るか
- 送信ボタンを disabled にしていないか: GOV.UK は、disabled のボタンは理由が分からず混乱を招くとしている。押せるようにして、押したときにエラーを示すほうがよい
- 必須か任意かが分かるか
- バックエンドとのつながり: サーバーのバリデーションエラー（Rails なら 422 と `errors`）を、項目ごとに画面の欄へ対応づけられる形で返しているか。フロントとサーバーで検証の内容が食い違っていないか

## 5. 知覚（見る・読む）

根拠: [WCAG 2.2](https://www.w3.org/WAI/WCAG22/Understanding/)（1.4.1 / 1.4.3 / 1.4.4 / 1.4.11 / 2.3.3）、[Practical Typography](https://practicaltypography.com/)、[web.dev Learn Design](https://web.dev/learn/design)

- コントラストが十分か: 本文は 4.5:1 以上、大きい文字は 3:1 以上（1.4.3）。ボタンの枠や入力欄の境界などの UI 部品は 3:1 以上（1.4.11）。実際の値は要実機確認
- 色だけで伝えていないか: エラーや完了を、色に加えて文字やアイコンでも示しているか（1.4.1）
- 拡大に耐えるか: 文字サイズを `px` で固定していないか。200% に拡大しても内容が欠けないか（1.4.4）
- 読みやすい行の長さか: 英語なら1行45〜90字（Practical Typography）。日本語は35〜40字程度が目安とされることが多い（推測・諸説あり）
- 動きを減らしたい人に配慮しているか: アニメーションが `prefers-reduced-motion` を尊重するか。`transition: all` を使っていないか（[Vercel Web Interface Guidelines](https://vercel.com/design/guidelines)）

## 6. 多様な人（障害・環境）

根拠: [web.dev Learn Accessibility](https://web.dev/learn/accessibility)、[Inclusive Components](https://inclusive-components.design/)、[freee アクセシビリティ・ガイドライン](https://a11y-guidelines.freee.co.jp/)、[WCAG 2.2 日本語訳](https://waic.jp/translations/WCAG22/)

- 構造が HTML で表されているか: 見出しの階層（h1 → h2 …）、一覧は `<ul>` / `<li>`、`<main>` / `<nav>` などのランドマーク
- 画像に代替テキストがあるか: 意味のある画像に `alt`。飾りの画像は `alt=""`。アイコンだけのボタンには `aria-label`
- 画面の変化が読み上げで伝わるか: 追加・削除・エラーなど、通信の結果で変わる内容に `aria-live`（または `role="status"` / `role="alert"`）があるか
- 言語の指定があるか: `<html lang="ja">`
- ARIA を使いすぎていないか: HTML の要素で表せるものに ARIA を足していないか（「間違った ARIA は ARIA なしより悪い」が APG の原則）

## 要実機確認のやり方（よく使うもの）

| 確認すること | やり方 |
|---|---|
| コントラスト | Chrome DevTools で要素を検査し、色の横に出るコントラスト比を見る |
| キーボード操作 | マウスを使わず Tab / Shift+Tab / Enter / Space / Esc だけで操作する |
| 待ち時間の体験 | DevTools の Network で通信を遅くする（Slow 4G など）。実験室の通信シミュレーターでもよい |
| 拡大 | ブラウザの拡大を 200% にする |
| 動きを減らす設定 | DevTools の Rendering で `prefers-reduced-motion: reduce` をエミュレートする |
| 全体の自動チェック | DevTools の Lighthouse でアクセシビリティを測る（自動で見つかるのは一部だけ） |
