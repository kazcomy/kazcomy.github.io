---
title: ブログ構成(Hugo+Github Pages+Sveltia CMS+Cloudflare Workers)
date: 2025-12-19T04:26:00
tags:
  - setup
draft: false
---
#### 選定基準
無料で使えることを優先してgithub pagesを選択
毎回git pushするのがだるい&出先やスマホから編集できるようCMSを導入 (OAuthで認証する)
当初Decap CMSを導入しようとしていたが、CMS側のバグ？かメッセージの期待値が不透明で10時間近く溶けたのでsveltia CMSを導入

#### OAuthの認証処理の流れ
```mermaid
sequenceDiagram
    participant User as ユーザー (Browser)
    participant CMS as Sveltia CMS (GitHub Pages)
    participant CF as Cloudflare Worker
    participant GH as GitHub (OAuth / API)

    Note over User, CMS: 1. ログイン操作
    User->>CMS: 管理画面アクセス (/admin)
    CMS->>User: 「Login with GitHub」ボタン表示
    User->>CF: 2. 認証リクエスト (/auth)
    CF->>GH: 3. GitHub認証画面へリダイレクト

    Note over User, GH: 4. ユーザー承認
    GH->>User: 認証許可を求める
    User->>GH: 承認 (Approve)
    GH->>CF: 5. コールバック (/callback?code=...)

    Note over CF, GH: 6. トークン交換
    CF->>GH: Client ID/Secret + Code を送信
    GH->>CF: Access Token を返却

    Note over CF, CMS: 7. トークン受け渡し
    CF->>CMS: postMessage等でトークンを送信
    CMS->>User: ログイン完了・管理画面表示

    Note over CMS, GH: 8. 記事投稿 (API)
    User->>CMS: 記事作成・保存
    CMS->>GH: GitHub APIでCommit & Push
    GH->>GH: GitHub Actions起動 (Hugo Build)
```
#### 導入方法
##### GitHub側設定
Settings > Developer settings > OAuth AppsからNew OAuth Appsを作成する。(Authorization callback URLにCloudflare Workersのcallbackを指定する)
Client IDとClient Secretをここで生成して控えておく

##### Cloudflare側設定
OAuth仲介用のWorker用コードを配置して環境変数として上記Cilent IDとSecretを指定する。現在は自作コードになっているがsveltia-cms-authというCloudflareデプロイ用のコードを公式が用意しているのでそれを使った方が楽
