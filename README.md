# GitHub Pull Shark アチーブメント自動収集ツール

このリポジトリは、GitHub の「Pull Shark」アチーブメントを自動的に取得するためのツールです。GitHub Actions を使用して、定期的にプルリクエストを作成・マージすることで、アチーブメントのティアを段階的に獲得できます。

## 仕組み

このツールは2つのワークフローで動作します：

1. **update.yaml（自動PR作成）**
   - 6分ごとに自動実行される（スケジュール実行）
   - `main.py` を実行して `status.md` のプルリクエスト数をカウントアップ
   - 変更内容を含むプルリクエストを自動作成

2. **merge.yaml（自動マージ）**
   - プルリクエストが作成されると自動的にトリガー
   - 作成者が設定したユーザー名と一致する場合、自動的にマージ
   - マージ完了により、Pull Shark のカウントが増加

## セットアップ方法

### 1. リポジトリをフォーク
このリポジトリを自分のアカウントにフォークしてください。

### 2. 初期値の設定
`status.md` ファイルを開き、現在のプルリクエスト数を入力します。
- 初めて使用する場合は `1` に設定
- 既に何件かマージ済みの場合は、その数値を入力

### 3. GitHub Actions の権限設定
GitHub Actions がプルリクエストを作成できるように、リポジトリの設定を変更します：

1. リポジトリの **Settings** > **Actions** > **General** に移動
2. **Workflow permissions** セクションで「**Allow GitHub Actions to create and approve pull requests**」を有効化

### 4. Personal Access Token (PAT) の作成と設定
GitHub Actions がプルリクエストをマージするために、Personal Access Token が必要です：

1. [Personal Access Token の作成ページ](https://docs.github.com/ja/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) にアクセス
2. 必要な権限（`repo` スコープ）を付与したトークンを作成
3. リポジトリの **Settings** > **Secrets and variables** > **Actions** に移動
4. **New repository secret** をクリック
5. Name に `PAT`、Value に作成したトークンを貼り付けて保存

### 5. ユーザー名の変更
`.github/workflows/merge.yaml` ファイルを開き、19行目の以下の部分を編集：

```yaml
if(creator != "ktsgsg")
```

`ktsgsg` を自分のGitHubユーザー名に変更してください。

### 6. GitHub Actions の有効化
1. リポジトリの **Actions** タブに移動
2. 「**I understand my workflows, go ahead and enable them**」をクリック

### 7. ワークフローの実行確認
- **Actions** タブで `achievement-collector` ワークフローが6分ごとに実行されることを確認
- プルリクエストが自動的に作成・マージされることを確認

### 8. アチーブメント達成後
すべてのティアを獲得したら、ワークフローを無効化することができます：
- **Actions** タブでワークフローを選択し、「**Disable workflow**」をクリック

## Pull Shark アチーブメントのティア

Pull Shark アチーブメントには、マージされたプルリクエストの数に応じて4つのティアがあります：

<table>
    <tr>
        <th>ティア名</th>
        <th>バッジ</th>
        <th>必要なマージ数</th>
    </tr>
    <tr>
        <td>Pull Shark デフォルト</td>
        <td><img src="images/pull-shark-default.png"></td>
        <td>2件以上</td>
    </tr>
    <tr>
        <td>Pull Shark ブロンズ</td>
        <td><img src="images/pull-shark-bronze.png"></td>
        <td>16件以上</td>
    </tr>
    <tr>
        <td>Pull Shark シルバー</td>
        <td><img src="images/pull-shark-silver.png"></td>
        <td>128件以上</td>
    </tr>
    <tr>
        <td>Pull Shark ゴールド</td>
        <td><img src="images/pull-shark-gold.png"></td>
        <td>1024件以上</td>
    </tr>
</table>

## 進捗状況の確認

`status.md` ファイルで現在の進捗状況と取得したバッジを確認できます。

## トラブルシューティング

### プルリクエストが自動作成されない
- GitHub Actions が有効になっているか確認
- `PAT` シークレットが正しく設定されているか確認
- ワークフローの権限設定を確認

### プルリクエストが自動マージされない
- `merge.yaml` のユーザー名が正しく設定されているか確認
- `PAT` に必要な権限（`repo`）が付与されているか確認

## 注意事項

- このツールは学習・実験目的で使用してください
- 過度な自動化はGitHubの利用規約に抵触する可能性があります
- 1024件に達したら、ワークフローを停止することをお勧めします

## コントリビューション

このリポジトリが役に立った場合は、⭐（スター）を付けていただけると嬉しいです！