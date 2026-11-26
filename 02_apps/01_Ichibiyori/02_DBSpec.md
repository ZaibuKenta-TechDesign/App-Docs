# Ichibiyori Backend

ASP.NET Core 8、Entity Framework Core、PostgreSQLで構成するAPIです。

## データベース構成

PostgreSQLの主な保存テーブルと関係です。

```mermaid
erDiagram
	AppUser {
		uuid Id PK
		string DisplayName
		string Email
		string NormalizedEmail UK
		string PasswordHash
		bytea ProfileImageData
		string ProfileImageContentType
		boolean IsAdmin
		datetime CreatedAtUtc
		datetime LastCommentNotificationReadAtUtc
		int LastCommentNotificationReadId
	}

	FishingCatch {
		int Id PK
		string UserId
		string Species
		double LengthCm
		int WeightGrams
		string Region
		string Location
		double Latitude
		double Longitude
		string Method
		string Tackle
		string Comment
		datetime CaughtAt
		string Visibility
		int Likes
	}

	CatchPhoto {
		int Id PK
		int FishingCatchId FK
		bytea ImageData
		string ContentType
		int DisplayOrder
	}

	CatchComment {
		int Id PK
		int CatchId FK
		uuid UserId FK
		string Body
		datetime CreatedAtUtc
	}

	CatchLike {
		uuid UserId PK, FK
		int CatchId PK, FK
		datetime CreatedAtUtc
	}

	SavedCatch {
		uuid UserId PK, FK
		int CatchId PK, FK
		datetime SavedAtUtc
	}

	HiddenCatch {
		uuid UserId PK, FK
		int CatchId PK, FK
		datetime HiddenAtUtc
	}

	UserFollow {
		uuid FollowerUserId PK, FK
		uuid FollowedUserId PK, FK
		datetime CreatedAtUtc
	}

	CatchReport {
		int Id PK
		int CatchId FK
		uuid ReporterUserId FK
		string Reason
		string Details
		datetime ReportedAtUtc
		string Status
	}

	FishingTrip {
		uuid UserId PK, FK
		date TripDate PK
		string Note
		datetime UpdatedAtUtc
	}

	TideLocation {
		int Id PK
		string Prefecture
		string AreaName
		double Latitude
		double Longitude
	}

	AppUser ||--o{ CatchComment : writes
	AppUser ||--o{ FishingCatch : owns_logically
	FishingCatch ||--o{ CatchComment : receives
	FishingCatch ||--o{ CatchPhoto : has
	AppUser ||--o{ CatchLike : likes
	FishingCatch ||--o{ CatchLike : receives
	AppUser ||--o{ SavedCatch : saves
	FishingCatch ||--o{ SavedCatch : saved_by
	AppUser ||--o{ HiddenCatch : hides
	FishingCatch ||--o{ HiddenCatch : hidden_by
	AppUser ||--o{ UserFollow : follower
	AppUser ||--o{ UserFollow : followed
	AppUser ||--o{ FishingTrip : records
	AppUser ||--o{ CatchReport : reports
	FishingCatch ||--o{ CatchReport : reported
```

- この図には永続エンティティ11個を示しています。`FishingCatch.UserId`は既存モデル上の文字列参照で、`AppUser.Id`へのDB外部キーではありません。その他の図示した関係はEFの外部キーです。
- `CatchPhoto.FishingCatchId`はEFが作るnullableなシャドウ外部キーで、C#モデルにはプロパティとして宣言されていません。
- `FishingCatch.IsMine`、`Liked`、`IsSaved`、`IsHidden`、`IsReportedByCurrentUser`、`Author`は`[NotMapped]`の応答用値で、DB列ではありません。
- `FishingCatch.Likes`は集計表示用カウンター、`CatchLike`はユーザーごとのいいね状態です。
- コメント通知は独立テーブルではなく、`CatchComment`と`AppUser.LastCommentNotificationReadAtUtc`/`LastCommentNotificationReadId`から算出します。
- `TideDetail`は潮汐API応答用モデルで、DBには保存しません。潮汐地点は`TideLocation`に保存します。
- 起動時のDB更新は`DatabaseInitializer`の冪等SQLを使用しています。現行モデルスナップショットには、`FishingTrip`、`CatchLike`、通知カーソル列がまだ反映されていないため、EFマイグレーションを生成・適用する場合はこの差を解消してください。