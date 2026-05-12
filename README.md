# The Graveyard of /p/

A site cataloguing the disappearance and transformation of the phoneme **/p/** across the world's languages.

> /p/ — the voiceless bilabial plosive — is among the most widespread phonemes in human language. Yet it is absent from roughly 10% of languages, and in many others it has weakened, shifted, or vanished entirely under specific phonological conditions. This project attempts to record those losses.

Hosted at **<https://p.languagepatterns.org>**.

## Stack

- [SvelteKit](https://svelte.dev/docs/kit) (Svelte 5)
- [`@sveltejs/adapter-cloudflare`](https://svelte.dev/docs/kit/adapter-cloudflare) — deployed as a Cloudflare Worker
- [Bun](https://bun.sh/) for package management and scripts

## Develop

```sh
bun install
bun run dev
```

## Build

```sh
bun run build
```

The build emits a Cloudflare Worker bundle into `.svelte-kit/cloudflare/`.

## Deploy

```sh
bun run build
bunx wrangler deploy
```

The custom domain (`p.languagepatterns.org`) is configured in [`wrangler.jsonc`](./wrangler.jsonc) via `routes` with `custom_domain: true`, so Cloudflare will provision the DNS record automatically if `languagepatterns.org` is a zone in the same account.

## Contributing

Sound changes are encoded with the `Notation` component:

```svelte
<Notation changes={['OJ: p', 'MJ: ɸ']} condition="V_V" />
```

— a list of stages and (optionally) the environment in which the change applies. New entries go in [`src/routes/+page.svelte`](./src/routes/+page.svelte).
