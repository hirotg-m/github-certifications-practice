# GH-200 GitHub Actions 実践練習計画

試験日: 2026-07-24（残り26日）  
現在の週: Week 2（6/23〜6/29）→ Week 3（6/30〜7/6）に移行中

---

## 作成するGitHubリポジトリ一覧

試験範囲をカバーする5つのリポジトリを作成して実際に動かす。

| # | リポジトリ名 | 学習テーマ | 優先度 |
|---|-------------|-----------|-------|
| 1 | `gh200-workflow-basics` | ワークフロー基礎・トリガー・YAML構文 | ★★★ |
| 2 | `gh200-ci-pipeline` | CI/CDパイプライン・マトリックスビルド | ★★★ |
| 3 | `gh200-custom-actions` | カスタムアクション作成（複合・JS・Docker） | ★★★ |
| 4 | `gh200-secrets-envs` | シークレット・環境変数・OIDCセキュリティ | ★★★ |
| 5 | `gh200-advanced` | セルフホステッドランナー・最適化・デプロイ | ★★☆ |

---

## リポジトリ別 練習内容

### Repo 1: `gh200-workflow-basics`
**目的**: ワークフローの基本構造とトリガーをすべて手を動かして確認する

```
.github/workflows/
├── 01-push-trigger.yml        # push, pull_request トリガー
├── 02-schedule-trigger.yml    # cron スケジュール実行
├── 03-manual-trigger.yml      # workflow_dispatch + inputs
├── 04-workflow-call.yml       # workflow_call (再利用可能ワークフロー)
├── 05-job-dependency.yml      # needs でジョブ依存関係
├── 06-conditions.yml          # if 条件式 (success, failure, always)
└── 07-env-context.yml         # contexts (github, env, secrets, runner)
```

**確認するポイント**:
- `on:` に複数トリガーを書く方法
- `paths:` / `branches:` フィルターの挙動
- `github.event_name` で実行元を判定する方法
- `GITHUB_TOKEN` の自動付与スコープ

---

### Repo 2: `gh200-ci-pipeline`
**目的**: 実際のCI/CDフローを構築してアーティファクト・キャッシュを体験する

```
.github/workflows/
├── 01-matrix-build.yml        # matrix (OS × Node version)
├── 02-cache-deps.yml          # actions/cache で npm/pip キャッシュ
├── 03-artifacts.yml           # upload-artifact / download-artifact
├── 04-test-report.yml         # テスト結果をSummaryに出力
└── 05-release.yml             # タグpushでリリース作成
```

**確認するポイント**:
- `strategy.matrix` の `include` / `exclude`
- `fail-fast: false` の意味
- キャッシュキーの設計（`hashFiles` 活用）
- アーティファクトの保持期間設定 (`retention-days`)
- `actions/upload-release-asset` vs `gh release upload`

---

### Repo 3: `gh200-custom-actions`
**目的**: 3種類のカスタムアクションをすべて自作して挙動の違いを理解する

```
actions/
├── composite-action/          # 複合アクション（最も試験に出やすい）
│   ├── action.yml
│   └── README.md
├── javascript-action/         # JavaScriptアクション
│   ├── action.yml
│   ├── index.js
│   └── package.json
└── docker-action/             # Dockerアクション
    ├── action.yml
    ├── Dockerfile
    └── entrypoint.sh

.github/workflows/
├── test-composite.yml
├── test-javascript.yml
└── test-docker.yml
```

**確認するポイント**:
- `action.yml` の `inputs` / `outputs` / `runs` 設定
- 複合アクションで `${{ inputs.xxx }}` を使う方法
- JSアクションで `@actions/core` を使って output をセット
- Dockerアクションの `branding` フィールド
- ローカルアクション参照 `uses: ./actions/composite-action`
- バージョンタグ参照 `uses: owner/repo@v1`

---

### Repo 4: `gh200-secrets-envs`
**目的**: 秘密情報の管理と権限制御を実際に設定して確認する

```
.github/workflows/
├── 01-secrets-usage.yml       # secrets.XXX の参照・マスキング確認
├── 02-env-scopes.yml          # env のスコープ（workflow / job / step）
├── 03-environment-deploy.yml  # Environments（production/staging）設定
├── 04-oidc-aws.yml            # OIDC でクラウド認証（概念確認）
└── 05-permissions.yml         # permissions: でトークン権限を絞る
```

**確認するポイント**:
- Repository / Organization / Environment レベルのシークレット優先順位
- `environment:` 設定での承認フロー（Required reviewers）
- `permissions:` ブロックの `contents: read` など最小権限の原則
- OIDC: `id-token: write` が必要な理由
- fork からの PR では secrets が渡らない理由と `pull_request_target` の使い分け

---

### Repo 5: `gh200-advanced`
**目的**: 試験の応用問題に対応するため高度なトピックを確認する

```
.github/workflows/
├── 01-concurrency.yml         # concurrency グループで二重実行防止
├── 02-timeout.yml             # timeout-minutes 設定
├── 03-self-hosted-sim.yml     # runs-on: [self-hosted, linux] の構文確認
├── 04-reusable-workflow.yml   # workflow_call で別リポジトリから呼び出し
└── 05-deployment-gates.yml    # wait-for-checks / deployment protection rules
```

**確認するポイント**:
- `concurrency.cancel-in-progress: true` の動作
- セルフホステッドランナーのラベル指定方法
- 再利用可能ワークフローの `secrets: inherit` vs 明示的渡し
- `actions/runner` スコープの違い（GitHub-hosted vs self-hosted）

---

## 残り26日間のスケジュール

### Phase 1: 基礎固め（6/28〜7/6）
| 日付 | やること |
|------|---------|
| **6/28〜6/29** | Repo 1 作成、ワークフロー基礎・トリガー・YAML構文を動かす |
| **6/30〜7/1** | Repo 2 作成、CIパイプライン・マトリックス・キャッシュを動かす |
| **7/2〜7/3** | Repo 3 作成、3種類のカスタムアクションをすべて自作 |
| **7/4〜7/6** | Microsoft Learn モジュールで基礎範囲を復習・穴埋め |

### Phase 2: 応用・実践（7/7〜7/17）
| 日付 | やること |
|------|---------|
| **7/7〜7/9** | Repo 4 作成、シークレット・Environments・OIDC・権限を確認 |
| **7/10〜7/12** | Repo 5 作成、並列制御・再利用ワークフロー・セルフホステッドランナー |
| **7/13〜7/15** | 実際のOSSリポジトリのワークフローを読んで構造を解析 |
| **7/16〜7/17** | 苦手分野の再実装・Microsoft Learn 模擬問題 |

### Phase 3: 総仕上げ（7/18〜7/24）
| 日付 | やること |
|------|---------|
| **7/18〜7/20** | 模擬問題を繰り返し、間違えた箇所を実際に動かして確認 |
| **7/21〜7/22** | 重要構文チートシートの暗記・弱点の最終確認 |
| **7/23** | 軽め復習のみ（詰め込み禁止） |
| **7/24** | **試験当日** |

---

## 試験でよく出る構文チートシート

```yaml
# ワークフロー必須構造
name: ワークフロー名
on:
  push:
    branches: [main]
    paths: ['src/**']
  workflow_dispatch:
    inputs:
      env:
        type: choice
        options: [staging, production]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write        # OIDC に必要
    environment: production  # 承認フロー
    env:
      NODE_ENV: production
    steps:
      - uses: actions/checkout@v4
      - name: ステップ名
        id: my-step
        run: echo "result=hello" >> $GITHUB_OUTPUT
      - name: 出力を使う
        run: echo "${{ steps.my-step.outputs.result }}"

  deploy:
    needs: build             # build が成功してから実行
    if: success()
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [18, 20]
        os: [ubuntu-latest, windows-latest]
      fail-fast: false
```

---

## 重要な覚え方

| 混同しやすい項目 | 正しい理解 |
|----------------|-----------|
| `env:` vs `secrets:` | env は平文でログに出る / secrets はマスクされる |
| `workflow_call` vs `workflow_dispatch` | call=別ワークフローから呼ぶ / dispatch=手動実行 |
| `upload-artifact` vs `cache` | artifact=ジョブ間・ワークフロー間の成果物保存 / cache=依存関係の高速化 |
| GitHub-hosted runner の無料枠 | public=無料 / private=月2000分まで |
| `pull_request` vs `pull_request_target` | target=forkからでもsecretsが使える（危険も伴う） |
