# Setup [`gh-xz`](https://github.com/hakadoriya/gh-xz)

GitHub Actions for setting up gh extension [`gh-xz`](https://github.com/hakadoriya/gh-xz).

## Notes

In GitHub Actions, `gh extension install` cannot be executed (due to the restricted GITHUB_TOKEN, git clone fails with 404).  
So, clone with `actions/checkout` and then move to the gh extension directory.  
ref: <https://github.com/cli/cli/discussions/6988>  

## Example

```yml
name: example

on:
  pull_request:

jobs:
  example:
    if: github.actor != 'dependabot[bot]' && github.actor != 'renovate[bot]'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: hakadoriya-actions/setup-gh-xz@v0.0.1
      - name: Run gh xz
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        shell: bash
        run: |
          gh xz put-anchored-comment ${{ github.repository }} ${{ github.event.number }} anchor1 "This anchored comment is posted by gh-xz."
```
