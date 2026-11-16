# devartslab-media

> Media asset host: public R2 bucket plus Worker serving share links with noindex headers.

DevArts Lab project. Agent conventions: [AGENTS.md](AGENTS.md).

Shareable file host for marketing assets (videos, images, documents)
distributed by link instead of uploaded to each external platform.

## Setup

- bash scripts/bootstrap.sh

## Behavior

- GET/HEAD only; objects served by key.
- `X-Robots-Tag: noindex, nofollow` on every response.
- `Cache-Control: public, max-age=86400`.
- HTTP range and conditional requests pass through (video scrubbing works).
- No directory listing; an object exists only to whoever has its URL.

## Deploy for your org

All org-specific values live in `wrangler.toml`. Edit four fields:

| Field                      | What to set                               |
| -------------------------- | ----------------------------------------- |
| `name`                     | Worker name                               |
| `routes[].pattern`         | Your hostname, e.g. `media.example.com/*` |
| `routes[].zone_name`       | Your zone, e.g. `example.com`             |
| `BRAND_NAME`               | Text served at `/`                        |
| `r2_buckets[].bucket_name` | R2 bucket name                            |

Then:

```
npx wrangler r2 bucket create <bucket-name>
npx wrangler deploy
npx wrangler r2 bucket dev-url enable <bucket-name>
```

`dev-url enable` gives a `pub-*.r2.dev` URL for testing before the custom
domain is ready.

## Upload and share

Dashboard: Cloudflare > R2 > `<bucket>` > Upload.

CLI (the `--remote` flag is required; without it wrangler writes to local
simulation state):

```
npx wrangler r2 object put <bucket>/<key> --file <local-path> --remote
```

Share link: `https://<your-domain>/<key>`.

## DevArts Lab deployment

- `https://media.devartslab.com/<key>` (production)
- `https://pub-8ef1b37e0d934faf8c2077bcfad63642.r2.dev/<key>` (testing)

## Support

[Open an issue](https://github.com/DevArtsLab/devartslab-media/issues).

## License

MIT. See [LICENSE](LICENSE).

## Credits

Built by [DevArts Lab](https://github.com/DevArtsLab).
