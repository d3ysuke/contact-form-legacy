# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

React 19 + Vite 製のお問い合わせフォーム（JavaScript / JSX、TypeScript なし）。バックエンドはなく、UI 文言とバリデーションメッセージはすべて日本語。

## コマンド

```bash
npm run dev      # 開発サーバー起動
npm run build    # 本番ビルド（dist/ に出力）
npm run preview  # ビルド結果のプレビュー
npm run lint     # ESLint
npm test         # テストを1回実行（vitest run、watch なし）

npx vitest run src/App.test.jsx   # ファイルを指定して実行
npx vitest run -t "テスト名の一部"  # テスト名で絞り込んで実行
npx vitest                        # watch モード
```

## アーキテクチャ

アプリのロジックはすべて `src/App.jsx` の `App` コンポーネント1つに収まっている。ルーターや状態管理ライブラリは使っていない。

- **画面遷移**: `isConfirm` と `isSubmitted` の2つの state による早期 return で、入力画面 → 確認画面 → 送信完了画面を切り替える。`isSubmitted` の判定が先に評価される。
- **フォーム状態**: `name` / `email` / `message` / `errors` を個別の `useState` で保持する。確認画面から「戻る」と入力値は維持され、完了画面の「新しいお問い合わせ」ボタンで全 state をリセットする。state を追加したらこのリセット処理（`App.jsx` のインラインの onClick）も更新すること。
- **バリデーション**: `validate()` が `handleConfirm`（フォーム送信時）にのみ実行され、エラーは `errors` に項目名をキーとして格納される。各項目の制約は 名前 2〜50 文字、メールは `@` と `.` を含むこと、内容 10〜1000 文字。入力欄の `type` は email 等ではなく `text` で、ブラウザ標準の検証には頼っていない。
- **送信処理**: `handleSubmit` はモックで、`console.log` に出力するだけ。API 通信は実装されていない。

スタイルは `src/App.css`（フォーム）と `src/index.css`（グローバル）のプレーン CSS。

## テスト

Vitest + React Testing Library。設定は `vite.config.js` の `test` セクションにあり（jsdom 環境、`globals: true`）、`src/setupTests.js` で jest-dom のマッチャーを読み込む。テストファイルはコンポーネントと同じ場所に `*.test.jsx` として置く。

## 既知の問題

`npm run lint` は現状 `src/App.test.jsx` の `describe` / `test` / `expect` で `no-undef` エラーになる。`eslint.config.js` の globals が `globals.browser` のみで、Vitest のグローバルが定義されていないため。`npm test` は通る。
