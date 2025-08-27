# GitHubプロフィールのセットアップガイド 📝

このガイドでは、GitHubプロフィールを作成し、カスタマイズした方法を説明します。

## 📋 作成したもの

1. **GitHubプロフィール** - 自己紹介とスキルを表示
2. **GitHub Metrics** - 活動統計を自動的に生成・更新

## 🚀 セットアップ手順

### ステップ1: プロフィールリポジトリの作成

GitHubプロフィールを表示するには、**自分のユーザー名と同じ名前のリポジトリ**を作成します。

1. GitHubで新しいリポジトリを作成
2. リポジトリ名: `kuronomagi`（あなたのユーザー名）
3. **Public**を選択（必須）
4. READMEは追加しない

### ステップ2: README.mdの作成

プロフィールの内容を`README.md`に記述しました。

#### 主な構成要素

```markdown
# 自己紹介
- 名前とキャッチフレーズ
- 現在の仕事内容
- 学習中の技術
- 所在地

# スキルセット
- プログラミング言語
- フレームワーク
- インフラ・クラウド
- ツール

# GitHub統計
- 言語使用状況
- コーディング習慣
```

### ステップ3: スキルアイコンの表示

[Skill Icons](https://skillicons.dev)を使用して、技術スタックを視覚的に表示：

```markdown
[![My Skills](https://skillicons.dev/icons?i=ruby,rails,ts,js,html,css)](https://skillicons.dev)
```

### ステップ4: GitHub Metricsの設定

#### 4-1. Personal Access Tokenの作成

1. GitHub Settings → Developer settings → Personal access tokens
2. 「Generate new token (classic)」をクリック
3. 必要な権限を選択：
   - ✅ `repo`（Privateリポジトリも含める場合）
   - ✅ `user:read`
4. 有効期限: 90日推奨
5. トークンをコピー（一度だけ表示）

#### 4-2. Secretの設定

1. プロフィールリポジトリの Settings → Secrets and variables → Actions
2. 「New repository secret」をクリック
3. Name: `METRICS_TOKEN`
4. Value: コピーしたトークンを貼り付け

#### 4-3. GitHub Actionsワークフローの作成

`.github/workflows/metrics.yml`ファイルを作成：

```yaml
name: Metrics
on:
  schedule: [{cron: "0 0 * * *"}]  # 毎日更新
  workflow_dispatch:  # 手動実行も可能
  push:
    branches: [main]  # プッシュ時も実行

jobs:
  github-metrics:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      # リポジトリをチェックアウト
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      # メトリクスを生成
      - uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          user: kuronomagi
          # その他の設定...
```

### ステップ5: コミットとプッシュ

```bash
# ファイルを追加
git add README.md
git add .github/workflows/metrics.yml

# コミット
git commit -m "[add] GitHubプロフィールとMetrics設定"

# プッシュ
git push origin main
```

## 📊 生成されるメトリクス

### 1. カレンダーメトリクス（`calendar-metrics.svg`）
- プロフィール情報
- 3Dコミットカレンダー
- コーディング習慣

### 2. 言語メトリクス（`language-metrics.svg`）
⚠️ 自分のリポジトリ以外がうまく反映されませんでした

- 最も使用している言語
- 最近使用した言語（365日間）
- 各言語の行数と割合

## ⚠️ よくある問題と解決方法

### 問題1: Metricsが表示されない
- **原因**: Personal Access Tokenの権限不足
- **解決**: `repo`権限があることを確認

### 問題2: "No push activity found"
- **原因**: リポジトリにコミット履歴がない
- **解決**: 何度かコミット・プッシュして履歴を作成

### 問題3: GitHub Actions失敗（409エラー）
- **原因**: ファイルの競合
- **解決**: `actions/checkout@v3`と`output_action: overwrite`を追加

## 🎨 カスタマイズのポイント

1. **スキルアイコン**: [Skill Icons](https://skillicons.dev)でサポートされているアイコンを確認
2. **バッジスタイル**: `for-the-badge`スタイルで統一感を出す
3. **セクション分け**: 見やすいように適切にカテゴリ分け
4. **Additional Skills**: アイコンがないスキルはテキストで記載

## 📚 参考リンク

- [GitHub Profile README](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme)
- [lowlighter/metrics](https://github.com/lowlighter/metrics)
- [Skill Icons](https://github.com/tandpfun/skill-icons)
- [Shields.io](https://shields.io/)

---
