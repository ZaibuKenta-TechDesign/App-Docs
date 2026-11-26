# App-Docs

個人開発および各種アプリケーション開発における**開発標準（スタンダード）**と、各アプリケーションの**設計・仕様ドキュメント**をまとめたリポジトリです。

ソースコード本体（プライベートリポジトリ管理を含む）の設計思想・品質担保の基準・テスト計画を可視化することを目的としています。

---

## 🛠️ 開発標準（`01_standards/`）

複数アプリケーションの品質と保守性を一定に保つための共通基準です。

* [`01_standards/`](./01_standards/) — 設計・コーディング規約、Git運用、セキュリティチェックリスト
*(※ `01_standards/` 内のファイルが増えたら、以下のように個別リンクを並べるのがおすすめです)*
* [`01_standards/coding-style.md`](./01_standards/coding-style.md) — 命名規則・コード品質基準

---

## 📱 アプリケーション別ドキュメント（`02_apps/`）

### 1. Ichibiyori

* **主な技術スタック:** [例：TypeScript / Next.js / Go / PostgreSQL / Docker]
* **ドキュメントパス:**
  * [`02_apps/01_Ichibiyori/01_Overview.md`](./02_apps/01_Ichibiyori/01_Overview.md) — システム概要・アーキテクチャ構成・技術選定
  * [`02_apps/01_Ichibiyori/02_DBSpec.md`](./02_apps/01_Ichibiyori/02_DBSpec.md) — DB設計・テーブル定義書
  * [`02_apps/01_Ichibiyori/03_BackTestSpec.md`](./02_apps/01_Ichibiyori/03_BackTestSpec.md) — バックエンド テスト仕様書
  * [`02_apps/01_Ichibiyori/04_FrontTestSpec.md`](./02_apps/01_Ichibiyori/04_FrontTestSpec.md) — フロントエンド テスト仕様書

---

## 🎯 このリポジトリで重視していること

* **実装前の設計と仕様の明文化:** 概要（Overview）からDB設計（DBSpec）までを事前に定義し、手戻りの少ない開発を行っています。
* **テスト観点を意識した品質担保:** バックエンド・フロントエンド双方のテスト仕様書（TestSpec）を作成し、正常系・異常系・境界値を網羅した実装を心がけています。
* **複数アプリを見据えた標準化:** 共通ルールを `01_standards` に切り出すことで、新規アプリ立ち上げ時のスピードと品質の両立を図っています。