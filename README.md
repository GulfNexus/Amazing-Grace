# Amazing-Grace Rooms for rent in Tampa and Largo Florida 

      - name: Cache Node modules
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-npm-${{ hashFiles('package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-npm-

name: Deploy Site
#
on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: write
  pages: write
  id-token: write

env:
  FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js 24
        uses: actions/setup-node@v4
        with:
          node-version: 24

      - name: Install dependencies
        run: npm ci --no-audit --no-fund

      - name: Build site
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
