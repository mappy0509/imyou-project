# I'M YOU PROJECT & NØW or EIGHT

1つのリポジトリで**2サイト**を配信している。

1. **I'M YOU PROJECT** (プロダクション公式サイト) — マルチページ
2. **NØW or EIGHT** (オーディション独立LP) — シングルページ／今後独自ドメインで本番公開予定

## ルーティング

### I'M YOU PROJECT（公式）
- `/` — Home (`index.html`)
- `/about` — プロジェクト概要 (`about.html`)
- `/members` — 第一期メンバー (`members.html`)
- `/cast` — 出演者情報 (`cast.html`)
- `/audition` — オーディション応募 (`audition.html`)
- `/contact` — お問い合わせ・SNS (`contact.html`)

### NØW or EIGHT（独立LP）
- `/now-or-eight` — LP 本体 (`now-or-eight.html`)
- `/audition-2026` — `/now-or-eight` への301リダイレクト（旧URL）

## 技術

- 静的 HTML / CSS / JavaScript（ビルド不要）
- Vercel にて `cleanUrls` で配信

## デプロイ

GitHub へ `git push` すると Vercel が自動デプロイ（vercel CLI の直接叩きは使わない）。

## 独自ドメイン運用メモ（NØW or EIGHT）

NØW or EIGHT 用に独自ドメインを取得する場合は以下の手順：

1. お名前ドットコムで取得（候補: `imyou-project.com` 等）
2. Vercel ダッシュボードで `imyou-project` プロジェクトにドメインを追加（`Settings > Domains`）
3. お名前ドットコム側で Vercel 指定の A/CNAME レコードを設定
4. SSL 発行後、**そのドメインの `/` で LP をルート表示したい場合**は `vercel.json` に以下の rewrites を追記：

```jsonc
{
  "rewrites": [
    {
      "source": "/:path((?!css|js|assets|media|.*\\.).*)?",
      "has": [{ "type": "host", "value": "YOUR-DOMAIN.com" }],
      "destination": "/now-or-eight"
    }
  ]
}
```

- `imyou-project.vercel.app` では今まで通り公式サイトがルートに残り、
- カスタムドメインでは LP (`now-or-eight.html`) がルートで表示される。
- `/css/*` `/assets/*` などの静的アセットはどちらのドメインからも正しく配信される。
