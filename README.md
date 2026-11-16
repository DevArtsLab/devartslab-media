# devartslab-media

> Media asset host: public R2 bucket plus Worker serving share links with noindex headers.

DevArts Lab project. Agent conventions: [AGENTS.md](AGENTS.md).

Shareable host for DevArts Lab marketing assets (videos, images, documents)
distributed by link instead of uploaded to each external platform.

## Endpoints

- `https://media.devartslab.com/<key>` (production)
- `https://pub-8ef1b37e0d934faf8c2077bcfad63642.r2.dev/<key>` (testing)

## Behavior

- GET/HEAD only; objects served by key.
- `X-Robots-Tag: noindex, nofollow` on every response.
- `Cache-Control: public, max-age=86400`.
- HTTP range and conditional requests pass through (video scrubbing works).
- No directory listing; an object exists only to whoever has its URL.

## Upload and share

Dashboard: Cloudflare > R2 > `devartslab-media` > Upload.

CLI (the `--remote` flag is required; without it wrangler writes to local
simulation state):

```
npx wrangler r2 object put devartslab-media/<key> --file <local-path> --remote
```

Share link: `https://media.devartslab.com/<key>`.

## Setup

- bash scripts/bootstrap.sh

## Ship

- npx wrangler deploy

Forking for another org: edit `name`, `routes[].pattern`, `routes[].zone_name`,
`BRAND_NAME`, `r2_buckets[].bucket_name` in `wrangler.toml`, create the bucket,
deploy. Full pattern: foundation repo, `guides/media-asset-hosting.md`.

## Support

[Open an issue](https://github.com/DevArtsLab/devartslab-media/issues).

## License

MIT. See [LICENSE](LICENSE).

## Credits

Built by [DevArts Lab](https://github.com/DevArtsLab).
