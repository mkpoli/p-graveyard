<script lang="ts">
	import Timeline from '$lib/Timeline.svelte';
	import Notation from '$lib/Notation.svelte';
	import logo from '../assets/logo.png';

	const title = 'The Graveyard of /p/';
	const description =
		"A phonological flow diagram of the voiceless bilabial plosive /p/ as it weakened, shifted, or vanished across language families — Indo-European, Semitic, Sinitic, Japonic.";
	const url = 'https://p.languagepatterns.org/';
</script>

<svelte:head>
	<title>{title}</title>
	<meta name="description" content={description} />
	<link rel="canonical" href={url} />

	<meta property="og:type" content="website" />
	<meta property="og:title" content={title} />
	<meta property="og:description" content={description} />
	<meta property="og:url" content={url} />
	<meta property="og:site_name" content="languagepatterns.org" />
	<meta property="og:locale" content="en" />

	<meta name="twitter:card" content="summary" />
	<meta name="twitter:title" content={title} />
	<meta name="twitter:description" content={description} />

	<meta
		name="keywords"
		content="historical linguistics, phonology, sound change, /p/, voiceless bilabial plosive, Old Japanese, Middle Chinese, Proto-Semitic, Proto-Indo-European, Proto-Celtic, comparative linguistics"
	/>
</svelte:head>

<header>
	<img src={logo} alt="" class="logo" />
	<h1>The Graveyard of /p/</h1>
	<p class="tagline">A record of where the voiceless bilabial plosive went to die.</p>
</header>

<main>
	<section class="quotes">
		<blockquote class="q-en">
			<span class="mark mark-open" aria-hidden="true">&ldquo;</span>
			<p>
				However language began, one thing is certain — it immediately began to change, and has
				been changing ever since.
			</p>
			<span class="mark mark-close" aria-hidden="true">&rdquo;</span>
			<cite>David Crystal · <span class="year">2007</span></cite>
		</blockquote>

		<blockquote lang="lzh" class="q-lzh">
			<p>
				時有古今。<br />
				地有南北。<br />
				字有更革。<br />
				音有轉移。
			</p>
			<cite>陳第 · <span class="year">1606</span></cite>
		</blockquote>
	</section>

	<section class="intro">
		<h2>What is /p/?</h2>
		<p>
			<code>/p/</code> — the
			<a href="https://en.wikipedia.org/wiki/Voiceless_bilabial_plosive">voiceless bilabial plosive</a
			>
			— is one of the most widespread phonemes in human language. And yet it is absent from roughly 10%
			of the world's languages, and in many others it has weakened, shifted, or vanished under specific
			phonological conditions.
		</p>
		<p>
			This site records those losses. The diagram below is a phonological flow chart — each ribbon
			is one lineage of /p/ over time. A ribbon's color is the phoneme as it's articulated at that
			year (amber for /p/, blue for /f/, violet for /ɸ/, green for /h/, teal for /ç/, rose for /w/,
			grey for ∅). When a single /p/ splits into multiple environments, ribbons fork. When the
			color shifts mid-flow, that's a sound change in progress. Pre-evidence sections fade in
			from transparent — we know /p/ was there, but its exact form is extrapolated. Dots are
			attestation points. The ribbons are grouped by language family so you can see, e.g., the
			fates of Indo-European *p side by side.
		</p>
		<p class="meta">
			Drag horizontally to pan; use the zoom controls to expand a period. Hover any ribbon or dot for
			details.
		</p>
	</section>

	<section class="timeline-wrap">
		<h2>Timeline</h2>
		<Timeline />
	</section>

	<section class="details">
		<h2>The Graveyard</h2>

		<article>
			<h3>Celtic</h3>
			<p class="status">Status: <span class="dead">Lost in Proto-Celtic</span></p>
			<p>
				One of the showpiece sound losses of Indo-European studies. PIE *p disappeared early in the
				Celtic branch, before Proto-Celtic split into its daughters. The classic cognates expose it:
			</p>
			<ul>
				<li><Notation changes={['PIE: *p', 'PCelt: ∅']} /> e.g. *ph₂tḗr → OIr. <i>athair</i></li>
				<li>Latin <i>piscis</i> ~ Old Irish <i>íasc</i> "fish"</li>
			</ul>
			<p>
				/p/ later re-entered Celtic from <i>kw</i> in the P-Celtic branch (Welsh <i>pedwar</i> "four"
				&lt; PIE *kʷetwores), but inherited PIE *p never came back.
			</p>
		</article>

		<article>
			<h3>Arabic</h3>
			<p class="status">Status: <span class="dead">Lost</span></p>
			<p>
				Proto-Semitic had *p, retained in many sister languages — Hebrew shows it as [p]~[f]
				allophony, Akkadian had /p/. Arabic merged it with /f/ before the historical record:
			</p>
			<ul>
				<li><Notation changes={['PSem: *p', 'Arabic: f']} /></li>
				<li>Modern loans: <i>park</i> → <i>bark</i> (بارك) or <i>fark</i> (فارك)</li>
			</ul>
		</article>

		<article>
			<h3>Chinese</h3>
			<p class="status">Status: <span class="alive">Partially alive</span></p>
			<p>
				Old Chinese had *p. In late Middle Chinese the labial initials split — 輕唇音 (light
				labials) and 重唇音 (heavy labials) — and *p underwent labiodentalization before specific
				medial-vowel combinations:
			</p>
			<ul>
				<li><Notation changes={['MC: p', 'Mand.: p']} condition="elsewhere" /> e.g. 北 <i>běi</i></li>
				<li>
					<Notation changes={['MC: p', 'Mand.: f']} condition="_jɨ, _ju, _jo" /> e.g. 飛 <i>fēi</i>,
					風 <i>fēng</i>
				</li>
			</ul>
			<p>
				Modern Mandarin keeps /p/ in plenty of words, but a large slice of the inherited vocabulary
				exited via /f/.
			</p>
		</article>

		<article>
			<h3>Japanese</h3>
			<p class="status">
				Status: <span class="alive">Conditioned loss</span> — /p/ survived in narrow environments
				and was later reinforced by loans.
			</p>
			<p>
				The most thoroughly documented case. Old Japanese /p/ weakened to /ɸ/ in most positions
				by the late Heian period, then split further by the following vowel. It was never
				eliminated outright — the {'{Q, N}_V'} branch carried /p/ through unchanged — and from
				the 16th century onward loanwords added /p/ back in unrestricted positions.
			</p>
			<ul>
				<li>
					<Notation changes={['OJ: p', 'MJ: ɸ']} condition="V_V" />
					<ul>
						<li>
							<Notation changes={['MJ: ɸ', 'mJ: h']} condition={'_{a, o, e}'} />
						</li>
						<li>
							<Notation changes={['MJ: ɸ', 'mJ: ç']} condition="_i" />
						</li>
						<li>
							<Notation changes={['MJ: ɸ', 'mJ: ɸ']} condition="_u" /> (marginal)
						</li>
					</ul>
				</li>
				<li>
					<Notation changes={['OJ: p', 'mJ: p']} condition={'{Q, N}_V'} /> e.g. 切符 <i>kippu</i>, 散歩
					<i>sanpo</i>
				</li>
			</ul>
			<p>
				/p/ then re-entered unrestricted positions wholesale: first via Portuguese loans (1543:
				<i>pan</i>, <i>tabako</i>), then Dutch, and at scale through modern English loans —
				reinforcing a phoneme that had narrowly survived the Heian-era weakening.
			</p>
		</article>

		<h2>References</h2>
		<p class="refs">
			Frellesvig, Bjarke (2010). <i>A History of the Japanese Language</i>. ·
			Baxter & Sagart (2014). <i>Old Chinese: A New Reconstruction</i>. ·
			Matasović (2009). <i>Etymological Dictionary of Proto-Celtic</i>. ·
			Lipiński (2001). <i>Semitic Languages: Outline of a Comparative Grammar</i>.
		</p>
	</section>
</main>

<footer>
	Part of <a href="https://languagepatterns.org">languagepatterns.org</a>.
</footer>

<style>
	:global(body) {
		background: #f9f5eb;
		color: #2a2620;
		font-family: 'Georgia', 'Cambria', serif;
		line-height: 1.55;
		margin: 0;
	}

	header {
		text-align: center;
		padding: 2.5em 1em 1em;
		border-bottom: 1px solid #d8d2c4;
		background: #fbf8f0;
	}
	header .logo {
		max-width: 96px;
		opacity: 0.75;
	}
	header h1 {
		margin: 0.2em 0 0;
		font-size: 2.4em;
		letter-spacing: 0.01em;
	}
	.tagline {
		color: #6b6357;
		font-style: italic;
		margin: 0.4em 0 0;
	}

	main {
		max-width: 1100px;
		margin: 0 auto;
		padding: 1.5em 1em 3em;
	}

	h2 {
		margin-top: 2.2em;
		padding-bottom: 0.3em;
		border-bottom: 1px solid #d8d2c4;
		font-weight: 500;
		letter-spacing: 0.02em;
	}

	.quotes {
		display: grid;
		grid-template-columns: 1fr;
		gap: 1.4em;
		margin: 2em 0 2.5em;
	}
	@media (min-width: 760px) {
		.quotes {
			grid-template-columns: 1fr 280px;
			align-items: stretch;
		}
	}

	blockquote {
		position: relative;
		margin: 0;
		background: linear-gradient(180deg, #fdfbf3 0%, #f5efde 100%);
		border: 1px solid #d8d2c4;
		border-radius: 2px;
		box-shadow:
			0 1px 0 #fff inset,
			0 6px 18px -10px rgba(60, 50, 30, 0.25),
			0 1px 2px rgba(60, 50, 30, 0.06);
	}

	.q-en {
		padding: 2em 2.4em 1.4em;
		font-family: 'Garamond', 'Georgia', 'Cambria', serif;
		font-size: 1.18em;
		line-height: 1.55;
		color: #2a2620;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	.q-en p {
		margin: 0;
		font-style: italic;
		text-indent: 0.4em;
	}
	.q-en .mark {
		position: absolute;
		font-family: 'Garamond', 'Georgia', serif;
		font-size: 5.5em;
		line-height: 1;
		color: #c08a3e;
		opacity: 0.45;
		font-style: normal;
		pointer-events: none;
		user-select: none;
	}
	.q-en .mark-open {
		top: -0.05em;
		left: 0.08em;
	}
	.q-en .mark-close {
		bottom: -0.42em;
		right: 0.18em;
	}

	.q-lzh {
		padding: 1.4em 1em 1.2em;
		font-family: 'Noto Serif CJK', 'Source Han Serif', 'Songti', 'MS Mincho', serif;
		font-size: 1.6em;
		line-height: 1.9;
		display: flex;
		flex-direction: row-reverse;
		justify-content: center;
		align-items: stretch;
		letter-spacing: 0.04em;
		background:
			radial-gradient(ellipse at 50% 0%, rgba(192, 138, 62, 0.06), transparent 70%),
			linear-gradient(180deg, #f5efde 0%, #ebe2c8 100%);
		border-color: #c4baa3;
	}
	.q-lzh p {
		writing-mode: vertical-rl;
		margin: 0;
		color: #2a2620;
	}
	.q-lzh cite {
		writing-mode: vertical-rl;
	}

	blockquote cite {
		display: block;
		margin-top: 0.8em;
		font-style: normal;
		font-weight: 500;
		font-size: 0.78em;
		font-family: ui-monospace, 'SFMono-Regular', Menlo, monospace;
		text-transform: uppercase;
		letter-spacing: 0.12em;
		color: #6b6357;
		text-align: right;
	}
	blockquote cite:before {
		content: '— ';
		color: #c08a3e;
	}
	blockquote cite .year {
		color: #2a2620;
	}
	.q-lzh cite {
		text-align: left;
		margin: 0 0.6em 0 0;
		font-size: 0.5em;
	}

	.intro p {
		font-size: 1.02em;
	}
	.intro p.meta {
		font-size: 0.92em;
		color: #6b6357;
		font-style: italic;
	}

	.timeline-wrap {
		margin-top: 2em;
	}

	.details article {
		margin: 1.6em 0;
		padding: 1em 1.2em;
		background: #fbf8f0;
		border: 1px solid #e6dfd0;
		border-radius: 6px;
	}
	.details article h3 {
		margin: 0 0 0.3em;
		font-size: 1.3em;
	}
	.status {
		font-size: 0.9em;
		color: #6b6357;
		margin: 0 0 0.6em;
	}
	.status .dead {
		color: #8a3d3d;
		font-weight: 600;
	}
	.status .alive {
		color: #2d7a3e;
		font-weight: 600;
	}

	ul {
		padding-left: 1.4em;
	}
	li {
		margin: 0.25em 0;
	}

	.refs {
		font-size: 0.9em;
		color: #6b6357;
		line-height: 1.7;
	}

	footer {
		margin-top: 3em;
		padding: 1.5em;
		text-align: center;
		font-size: 0.9em;
		color: #6b6357;
		border-top: 1px solid #d8d2c4;
		background: #fbf8f0;
	}
	footer a {
		color: #c08a3e;
	}
</style>
