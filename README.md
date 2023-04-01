# Github Action: Dependabot auto merge
Merge dependabot PR by auto merge

# 利用の方法
1. こちらの[リンク](https://docs.github.com/ja/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%AE-github-actions-%E6%A8%A9%E9%99%90%E3%82%92%E7%AE%A1%E7%90%86%E3%81%99%E3%82%8B)先の内容にしたがって、actionの設定画面より下記のオプションを有効にしてください。
  ```
  [Allow OWNER, and select non-OWNER, actions and reusable workflows]`
  ```
2. こちらの[リンク](https://docs.github.com/ja/code-security/dependabot/dependabot-version-updates/configuring-dependabot-version-updates#dependabot-version-updates-%E3%82%92%E6%9C%89%E5%8A%B9%E5%8C%96%E3%81%99%E3%82%8B)先の内容にしたがって、dependabotを有効化してください。
3. `.github/dependabot.yml`を配置し、dependabotを有効化して下さい。<br>
e,g) npm
  ```yaml
  version: 2
  updates:
  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: daily
      time: "20:00"
    open-pull-requests-limit: 10
  ```

4. `.github/workflow/` 配下に下記内容のyamlファイルを配置して下さい。
```yaml
name: Dependabot auto-merge
on: pull_request_target

permissions:
  pull-requests: write
  contents: write

jobs:
  dependabot:
    if: ${{ github.actor == 'dependabot[bot]' }}
    runs-on: ubuntu-latest
    steps:
      - name: auto merge
        uses: metaps/action-dependabot-auto-merge@main
        with:
          merge-method: "squash"
          github-token: "${{ secrets.GITHUB_TOKEN }}"
```


### Inputs

| input          | required | default                  | description                                         |
|----------------|----------|--------------------------|-----------------------------------------------------|
| `github-token` | ✔        |                          | mergeに利用するgithub tokenを指定します                 |
| `merge-method` | ❌       | `merge`                  | マージ方法を指定します. (squash,rebase,merge)           |
