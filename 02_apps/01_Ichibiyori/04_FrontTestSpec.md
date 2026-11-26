# 一尾日和 フロントエンド

Next.js（App Router）、React、TypeScriptで構成した釣果共有アプリのフロントエンドです。

## テスト

### Vitest / React Testing Library

jsdom上でReactコンポーネントとサービス関数をテストします。API通信はMSWでモックするため、バックエンドやPostgreSQLを起動せずに実行できます。

```powershell
npm test
```

特定のテストファイルだけ実行する場合:

```powershell
npm test -- src/test/NotificationBell.test.tsx
```

テストは`src/test`にあり、APIの送受信・エラー処理、通知ベル、フォロー中フィード、釣行カレンダーの操作などを確認します。

### Playwright E2E

ChromiumでNext.jsを起動し、ログインからフォロー中フィードへの切り替えをブラウザー上で確認します。API応答はテスト内でモックするため、バックエンドやDBは不要です。初回のみChromiumをインストールします。

```powershell
npx playwright install chromium
npm run test:e2e
```

Playwrightはテスト時にNext.js開発サーバーを`http://127.0.0.1:3167`で自動起動します。失敗時のスクリーンショットとトレースは`test-results`に保存されます。

### 品質チェック

```powershell
npm run lint
npm run build
```

`npm run build`はTypeScript検査とNext.js本番ビルドを行います。lintは既存画面に関する警告を報告する場合があります。
