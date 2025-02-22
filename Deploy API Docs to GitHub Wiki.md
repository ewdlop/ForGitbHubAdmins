name: Deploy API Docs to GitHub Wiki
on:
  push:
    paths:
      - 'api-specification.md'
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy-wiki:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v3

      - name: Get current date
        id: date
        run: echo "date=$(date +'%Y-%m-%d %H:%M:%S')" >> $GITHUB_OUTPUT

      - name: Update timestamp in API spec
        run: |
          sed -i "s/Last Updated:.*/Last Updated: ${{ steps.date.outputs.date }} UTC/" api-specification.md

      - name: Push to Wiki
        uses: Andrew-Chen-Wang/github-wiki-action@v4
        env:
          # Make sure to create a token with repo scope
          WIKI_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          WIKI_ACTOR: ewdlop
        with:
          path: 'api-specification.md'
          commit-message: "Update API documentation: ${{ steps.date.outputs.date }}"
