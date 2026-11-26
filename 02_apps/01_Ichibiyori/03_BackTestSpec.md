# Ichibiyori Backend

ASP.NET Core 8、Entity Framework Core、PostgreSQLで構成するAPIです。

## テスト

バックエンドのコントローラーテストは`Ichibiyori.Tests`にあり、xUnitとEF Core InMemory Providerを使用します。PostgreSQLやAPIサーバーを起動せず、テストごとに独立したインメモリDBで実行できます。

このディレクトリで実行:

```powershell
dotnet test Ichibiyori.sln --configuration Release
```

現在のテスト範囲:

- フォロー中フィードが未フォロー相手・非公開投稿を除外し、地域公開投稿の正確な座標を隠すこと
- 地図APIが「詳細を公開」かつ座標付きの釣果だけを返すこと
- コメント通知が他人から自分の釣果への未読コメントだけを数え、既読時刻更新後に未読数が減ること
- 釣行メモが保存したユーザー本人にだけ返ること

テストはコントローラーを直接呼び出す単体テストです。ASP.NET Coreの認証ミドルウェア、HTTPパイプライン、実PostgreSQLとの接続を検証する統合テストではありません。認証ユーザーはテスト内でClaimsとして設定しています。