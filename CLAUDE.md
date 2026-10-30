# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

React製の日本語お問い合わせフォーム。入力ー確認ー送信完了の画面遷移を持つSPA。
バリデーション（名前・メールアドレス・メッセージ）を含む。送信処理はモック(console.log)。

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

単一コンポーネント構成。'src/App.jsx'にフォームのロジック・バリデーション・画面遷移が全て集約されている。
状態管理は'useState'のみ。
コンポーネント分割やカスタムフック抽出は未実装。

## コーディングルール

-変更後は必ず、'npx vitest run'でテストが通ることを確認する。
-変更は1つの関心事に絞り、小さい単位で行う。
-依頼された範囲以外のコードは変更しない。