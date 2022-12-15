# Github Action: Dependabot auto merge
Merge dependabot PR by auto merge

# 利用の方法
```
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
      - name: Dependabot metadata
        id: dependabot-metadata
        uses: metaps/action-dependabot-auto-merge@main
        with:
          merge-method: "squash"
          github-token: "${{ secrets.GITHUB_TOKEN }}"
```
`.github/workflow/`配下に上記のようなyamlファイルを配置してください

### Inputs

| input          | required | default                  | description                                         |
|----------------|----------|--------------------------|-----------------------------------------------------|
| `github-token` | ✔        | `github.token`           | mergeに利用するgithub tokenを指定します                 |
| `merge-method` | ❌       | `merge`                  | マージ方法を指定します. (squash,rebase,merge)           |
