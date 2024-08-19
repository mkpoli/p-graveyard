<script lang="ts">
	import Notation from '$lib/Notation.svelte';
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

	<h2>The Graveyard</h2>

	<h3>Japanese</h3>

	<p>~7c [citation needed]; Middle Japanese lost common /p/ almost completely.</p>
	<h4><abbr title="Old Japanese">OJ</abbr> /p/</h4>
	<p>Status: Marginally Alive</p>
	<ul>
		<li>
			<Notation changes={['OJ: p', 'MJ: ɸ']} condition="V_V" />
			<ul>
				<li>
					<Notation changes={['MJ~EmJ: ɸ', 'mJ: ɸ']} condition={'#_u'} />
				</li>
				<li>
					<Notation changes={['MJ~EmJ: ɸ', 'mJ: h']} condition={'#_{a,o}'} />
				</li>
				<li>
					<Notation changes={['MJ~EmJ: ɸ', 'mJ: ç']} condition={'#_i'} />
				</li>
			</ul>
		</li>
		<li>
			<Notation changes={['OJ: p', 'MJ: w']} condition="V_V" />
		</li>
		<li>
			<Notation changes={['OJ: p', 'mJ: p']} condition={'{N,R}_V'} />
		</li>
		<li>
			Borrowed
			<ul>
				<li><Notation changes={['MC: p-', 'MJ: ɸ']} /></li>
			</ul>
			<ul>
				<li><Notation changes={['MC: -p', 'MJ: ɸu']} /></li>
			</ul>
		</li>
		<li>
			Onomatopeic
			<ul>
				<li>e.g. /pikapika/ &lt; /pikari/ &gt; /hikari/</li>
			</ul>
		</li>
	</ul>

	<h4><abbr title="Early Modern Japanese">EmJ /p/</abbr></h4>
	<p>Status: Alive</p>
	<ul>
		<li>Borrowed: /p/</li>
	</ul>

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

	ul {
		text-align: left;

		margin: 0 auto;
		width: fit-content;
	}

	li {
		width: fit-content;
	}
</style>
