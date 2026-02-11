# BOX-Sharing

Box APIを使用したセキュアなファイルアップロード・共有ウェブアプリケーション。

## 概要

BOX-Sharingは、Flask と Box SDK を使用したファイルアップロードシステムです。ユーザーがファイルをアップロードすると、自動的にBox内にランダムに生成されたフォルダが作成され、そこにファイルが保存されます。その後、共有リンクが生成されます。

## 機能

- **Box OAuth2認証**: セキュアな認証フロー
- **ファイルアップロード**: ウェブUIからのドラッグ&ドロップ対応
- **自動フォルダ生成**: ランダムに生成されたフォルダ名でファイルを整理
- **共有リンク生成**: アップロード後、共有リンクを自動生成
- **一時ファイル管理**: アップロード後、サーバー上の一時ファイルを自動削除

## 必要な環境

- Python 3.8 以上
- Box Developer Account

## インストール

1. リポジトリをクローン
```bash
git clone https://github.com/yourusername/BOX-Sharing.git
cd BOX-Sharing
```

2. 仮想環境を作成・有効化
```bash
python -m venv venv
venv\Scripts\activate  # Windows
source venv/bin/activate  # macOS/Linux
```

3. 必要なパッケージをインストール
```bash
pip install flask boxsdk requests
```

4. 環境設定ファイルを作成
```bash
copy env_sample.ini env.ini
```

5. `env.ini` ファイルを編集して、Box Developer Consoleから取得した認証情報を入力
```ini
[box]
client_id = your_client_id
client_secret = your_client_secret
folder_id = your_upload_folder_id
```

## Box Developer Consoleでの設定

1. [Box Developer Console](https://app.box.com/developers/console) にアクセス
2. 新しいアプリケーションを作成
3. OAuth 2.0認証を選択
4. リダイレクトURIに `http://localhost:5000/callback` を設定
5. Client ID と Client Secret をコピーして `env.ini` に入力
6. Box内でアップロード先フォルダを作成し、フォルダIDを取得して `env.ini` に入力

## 使用方法

1. アプリケーションを起動
```bash
python main.py
```

2. ブラウザで `http://localhost:5000` にアクセス

3. 初回アクセス時は、Box OAuth画面にリダイレクトされます
   - Box アカウントでログイン
   - アプリケーションに必要な権限を許可

4. ファイルアップロードページで、ファイルを選択してアップロード

5. アップロード完了後、共有リンクが表示されます

## ファイル構成

```
BOX-Sharing/
├── main.py                 # メインアプリケーション
├── env_sample.ini          # 環境設定ファイルのサンプル
├── env.ini                 # 環境設定ファイル（.gitignoreに追加推奨）
├── templates/
│   ├── upload.html         # ファイルアップロード画面
│   └── result.html         # アップロード完了画面
└── tmp/                    # 一時ファイル保存ディレクトリ
    └── dummy               # プレースホルダー
```

## セキュリティ上の注意

- `env.ini` ファイルには認証情報が含まれているため、**絶対にGitにコミットしないでください**
- `.gitignore` に `env.ini` を追加してください
- Client Secret は安全に管理してください
- 本番環境では環境変数を使用することをお勧めします

## 環境変数の使用（本番環境推奨）

```python
import os

CLIENT_ID = os.getenv('BOX_CLIENT_ID')
CLIENT_SECRET = os.getenv('BOX_CLIENT_SECRET')
FOLDER_ID = os.getenv('BOX_FOLDER_ID')
```

## 主な関数

### `generate_password(length=8)`
- ランダムなパスワード文字列を生成
- デフォルトは8文字
- セキュアではない文字（0, O, I, i, l, 1など）を除外

### `store_tokens(access_token, refresh_token)`
- アクセストークンとリフレッシュトークンを保存

### `read_tokens()`
- 保存されたトークンを読み込み

## トラブルシューティング

### `env.ini not found` エラーが出る
- `env.ini` ファイルが存在することを確認してください

### OAuth認証エラー
- Client ID と Client Secret が正しいことを確認
- リダイレクトURIが `http://localhost:5000/callback` に設定されていることを確認

### ファイルアップロードエラー
- `tmp/` ディレクトリが存在することを確認
- フォルダIDが正しいことを確認

## ライセンス

MIT License

## 貢献

プルリクエストを歓迎します。大きな変更の場合は、まずissueを開いて変更内容を説明してください。

## サポート

問題が発生した場合は、[Issues](https://github.com/yourusername/BOX-Sharing/issues) で報告してください。
