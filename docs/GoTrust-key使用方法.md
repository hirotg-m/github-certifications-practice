# GoTrust社の Idem Key で GitHub にログインする方法

## 前提
1. GoTrust Idem Key の初期設定（PIN設定など）が完了している。
1. Windows Hello が有効で、対応ブラウザ（Edge / Chrome / Firefox）を利用できる。
1. GitHub アカウントにログイン済み（初回登録時のみ）。

## 1. GitHub に Idem Key を登録する
1. GitHub 右上プロフィール画像から `Settings` を開く。
1. 左メニュー `Password and authentication` を開く。
1. `Two-factor authentication` セクションの `Passkeys and security keys` で `Add` を押す。
1. ブラウザの案内で `Security key` を選ぶ。
1. Idem Key を USB に挿し、必要に応じてボタンをタッチする。
1. PIN 入力を求められた場合は、Idem Key の PIN を入力する。
1. 表示名（例: `IdemKey-main`）を付けて保存する。

## 2. ログイン時に Idem Key を使う
1. GitHub のログイン画面でユーザー名入力後、`Use a passkey` または `Use security key` を選ぶ。
1. 登録済みの Idem Key を挿した状態で、ブラウザの認証ダイアログを進める。
1. 必要に応じて PIN 入力とタッチ操作を行う。
1. 認証成功後、GitHub にログインできる。

## 3. 併せてやっておくべき設定（推奨）
1. バックアップ用の認証手段を必ず追加する。
	 - 予備のセキュリティキー
	 - 認証アプリ（TOTP）
	 - GitHub の recovery codes
1. recovery codes はオフラインでも参照できる場所に保管する。
1. Idem Key を紛失した場合に備え、登録済みキーの名前を分かりやすくしておく。

## 4. よくあるつまずき
- キーが認識されない:
	- 別の USB ポートに挿し直す。
	- ブラウザを最新化し、別ブラウザでも試す。
	- 企業PCの場合、USBセキュリティ制限がないか確認する。
- PIN エラーが出る:
	- PIN の入力ミス回数に注意する（規定回数超過でロックの可能性あり）。
- `Passkey` が表示されない:
	- GitHub の `Password and authentication` 画面で、`Passkeys and security keys` から追加する。

## 5. メモ
- GitHub への認証はブラウザでのサインイン時に有効。
- `git clone` や `git push` で HTTPS を使う場合は、PAT（Personal Access Token）または GitHub CLI 連携が必要。
