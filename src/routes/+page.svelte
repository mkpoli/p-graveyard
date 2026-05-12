<script lang="ts">
	import Timeline from '$lib/Timeline.svelte';
	import Notation from '$lib/Notation.svelte';
	import logo from '../assets/logo.png';
</script>

<header>
	<img src={logo} alt="" class="logo" />
	<h1>The Graveyard of /p/</h1>
	<p class="tagline">A record of where the voiceless bilabial plosive went to die.</p>
</header>

<main>
	<section class="quotes">
		<blockquote>
			However language began, one thing is certain — it immediately began to change, and has been
			changing ever since.<cite>David Crystal (2007)</cite>
		</blockquote>

		<blockquote lang="lzh">
			時有古今。<br />
			地有南北。<br />
			字有更革。<br />
			音有轉移。<cite>陳第（1606）</cite>
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
			This site records those losses. Each row in the timeline below is one language's /p/ over
			time. Solid amber means /p/ was still articulated as <code>[p]</code>; the gradient is the
			interpolated window during which it shifted to something else; the cool grey is the
			post-transition state. Splits in a row mark conditioned changes — when a single /p/ broke into
			multiple environments and only some of those lineages survived. Dots are evidence points
			(attestations or reconstructions); striped fills are extrapolated beyond the evidence.
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
			<p class="status">Status: <span class="alive">Resurrected</span></p>
			<p>
				The most thoroughly documented case. Old Japanese /p/ weakened to /ɸ/ across most positions
				by the late Heian period, then split further by the following vowel:
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
				/p/ then re-entered the language wholesale: first via Portuguese loans (1543: <i>pan</i>,
				<i>tabako</i>), then Dutch, and at scale through modern English loans.
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
		gap: 1em;
		margin: 1.5em 0;
	}
	@media (min-width: 720px) {
		.quotes {
			grid-template-columns: 1fr auto;
		}
	}
	blockquote {
		margin: 0;
		padding: 0.8em 1.2em;
		background: #fbf8f0;
		border-left: 4px solid #c08a3e;
		border-radius: 4px;
		font-style: italic;
		font-size: 1.05em;
	}
	blockquote cite {
		display: block;
		margin-top: 0.4em;
		font-style: normal;
		font-weight: 600;
		text-align: right;
		font-size: 0.9em;
	}
	blockquote cite:before {
		content: '— ';
	}
	blockquote[lang='lzh'] {
		writing-mode: vertical-rl;
		font-style: normal;
		border-left: none;
		border-right: 4px solid #5a6470;
		padding: 1em 0.8em;
		font-size: 1.1em;
	}

	.intro p {
		font-size: 1.02em;
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
