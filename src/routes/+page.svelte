<script lang="ts">
	import logo from '../assets/logo.png';
</script>

<h1>The Graveyard of /p/</h1>

<main>
	<div class="quotes">
		<blockquote>
			However language began, one thing is certain – it immediately began to change, and has been
			changing ever since. <cite>David Crystal (2007)</cite>
		</blockquote>

		<blockquote lang="lzh">
			時有古今。<br />
			地有南北。<br />
			字有更革。<br />
			音有轉移。<br /><cite>陳第（1606）</cite>
		</blockquote>
	</div>

	<img src={logo} alt="graveyard" />

	<h2>What is /p/?</h2>
	<p>
		<code>/p/</code> as IPA notation of the
		<a href="https://en.wikipedia.org/wiki/Voiceless_bilabial_plosive">voiceless bilabial plosive</a
		>
		consonant, is one of, if not the most common phoneme in all human languages [citation needed]. However,
		this phoneme is missing in around 10% of languages, and many languages have the sound disappeared
		once or systematically lost in specific environments [citation needed].
	</p>

	<p>This site is a project that attempts to record all disappeared or transformed poor /p/ s.</p>

	<h2>References</h2>
</main>

<style>
	main {
		padding: 0.5em;

		text-align: center;
		margin: 0 auto;
		max-width: 1024px;
	}

	h1 {
		text-align: center;
	}

	.quotes {
		display: flex;
		justify-content: space-evenly;

		margin-bottom: 1em;
		gap: 1em;
	}

	blockquote {
		padding: 0.5em 1em;

		background-color: #f9f9f9;

		font-style: italic;
		font-size: 1.2em;

		border-radius: 0.5em;

		margin: 0 0 1em 0;
		border-left: 0.5em solid #ccc;
	}

	blockquote cite {
		font-weight: bold;
		font-style: normal;

		display: block;
		text-align: right;
	}

	blockquote[lang='lzh'] {
		writing-mode: vertical-rl;
		font-style: normal;
	}

	blockquote cite:before {
		content: '— ';
	}
</style>
