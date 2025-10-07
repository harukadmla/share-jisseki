# オーナーアカウント クイックセットアップガイド

## 📝 手順

### 1. アカウントの作成

ブラウザで `http://localhost:3001/auth/signup` にアクセスして、以下の情報で登録：

- **表示名**: オーナー
- **メールアドレス**: `harukadmla@gmail.com`
- **パスワード**: `Pw321321`
- **パスワード（確認）**: `Pw321321`
- **申請する権限**: 管理者

登録後、「アカウント作成の申請完了」メッセージが表示されます。

### 2. Supabaseダッシュボードでオーナー権限を付与

1. [Supabaseダッシュボード](https://supabase.com/dashboard)にログイン
2. プロジェクトを選択
3. **SQL Editor** に移動
4. 以下のSQLを実行：

```sql
-- オーナーアカウントの権限を設定
SELECT public.setup_owner_account();
```

このコマンドで以下が実行されます：
- `harukadmla@gmail.com` のアカウントを探す
- roleを `owner` に設定
- statusを `approved` に設定
- 表示名を「オーナー」に設定

### 3. ログインテスト

1. ブラウザで `http://localhost:3001/auth/login` にアクセス
2. 以下でログイン：
   - **メールアドレス**: `harukadmla@gmail.com`
   - **パスワード**: `Pw321321`

## 🔍 トラブルシューティング

### "Invalid login credentials" エラーの場合

Supabaseダッシュボードで直接ユーザーを作成：

1. **Authentication** > **Users** に移動
2. **Add user** > **Create new user** をクリック
3. 以下を入力：
   - Email: `harukadmla@gmail.com`
   - Password: `Pw321321`
   - Auto Confirm User: ✅ チェック
4. **Create user** をクリック
5. SQL Editorで `SELECT public.setup_owner_account();` を実行

### プロファイルの確認

SQL Editorで以下を実行して、オーナーアカウントの状態を確認：

```sql
SELECT 
  u.id,
  u.email,
  p.display_name,
  p.role,
  p.status
FROM auth.users u
LEFT JOIN public.profiles p ON u.id = p.id
WHERE u.email = 'harukadmla@gmail.com';
```

期待される結果：
- email: `harukadmla@gmail.com`
- display_name: `オーナー`
- role: `owner`
- status: `approved`

## 🚀 成功確認

ログイン後、ナビゲーションバーに以下が表示されれば成功：
- アカウント名: **オーナー**
- 役職: **オーナー**
- ログアウトボタン