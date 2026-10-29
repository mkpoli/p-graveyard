<script lang="ts">
	// The header's Sponsor pill, led by the author's face, and the dialog it opens: her portrait,
	// why she needs help, then three steps that end at the Ko-fi button.
	const text = {
		label: 'Sponsor',
		close: 'Close',
		lead: "I make The Graveyard of /p/ on my own, out of love for languages, and I pay for it myself. I don't have a job, and my small TA pay doesn't cover rent and food. Some days I can barely afford to eat.",
		ask: 'Please help me keep going 🥺 Even one dollar helps me eat today.',
		howTo: 'How to help',
		steps: [
			'Tap Ko-fi below.',
			'Pick any amount. Small is fine.',
			'Pay with PayPal or a card. No sign-up needed.'
		],
		kofi: 'One-time or monthly',
		company: 'Company sponsorship',
		companyHint: "I'd love to put your ad on this site. Please write to mkpoli@mkpo.li.",
		thanks:
			'Thank you for reading this far. Your help keeps me fed and keeps The Graveyard of /p/ growing 💗'
	};

	let open = $state(false);
	let dialog = $state<HTMLDialogElement>();

	$effect(() => {
		if (open && dialog && !dialog.open) dialog.showModal();
	});

	// The dialog lives under <body>, away from the header's stacking and wrapping.
	function portal(node: HTMLElement) {
		document.body.append(node);
		return { destroy: () => node.remove() };
	}

	// A click on the backdrop closes the dialog; one on its own scrollbar also targets the dialog,
	// so only a click outside its box counts.
	function outside(event: MouseEvent) {
		if (!dialog || event.target !== dialog) return;
		const box = dialog.getBoundingClientRect();
		const { clientX: x, clientY: y } = event;
		if (x < box.left || x > box.right || y < box.top || y > box.bottom) dialog.close();
	}

	const heart =
		'M12 21s-8.5-5.1-8.5-11.3A4.7 4.7 0 0 1 12 6.9a4.7 4.7 0 0 1 8.5 2.8C20.5 15.9 12 21 12 21Z';
</script>

<button
	type="button"
	class="sponsor"
	aria-label={text.label}
	aria-haspopup="dialog"
	onclick={() => (open = true)}
>
	<span class="face" aria-hidden="true">
		<img src="/sponsor-face.webp" alt="" width="26" height="26" decoding="async" />
		<svg class="face-heart" viewBox="0 0 24 24"><path d={heart} /></svg>
	</span>
	<span class="label">{text.label}</span>
</button>

{#if open}
	<dialog
		class="sponsor-dialog"
		bind:this={dialog}
		use:portal
		aria-labelledby="sponsor-title"
		onclose={() => (open = false)}
		onclick={outside}
	>
		<div class="top">
			<h2 id="sponsor-title">{text.label}</h2>
			<button class="dismiss" type="button" aria-label={text.close} onclick={() => dialog?.close()}>
				<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6 6l12 12M18 6L6 18" /></svg>
			</button>
		</div>
		<div class="sheet">
			<div class="hero" aria-hidden="true">
				<span class="portrait">
					<img src="/sponsor-portrait.webp" alt="" width="112" height="112" decoding="async" />
					<svg class="heart" viewBox="0 0 24 24">
						<path d={heart} />
						<ellipse
							class="shine"
							cx="7.6"
							cy="9.6"
							rx="1.5"
							ry="2.1"
							transform="rotate(-30 7.6 9.6)"
						/>
					</svg>
				</span>
				<span class="spark s1">✦</span><span class="spark s2">✦</span><span class="spark s3">✦</span
				>
			</div>
			<p class="lead">{text.lead}</p>
			<p class="ask">{text.ask}</p>
			<section class="how">
				<h3>{text.howTo}</h3>
				<ol class="steps" role="list">
					{#each text.steps as step (step)}<li>{step}</li>{/each}
				</ol>
			</section>
			<a class="kofi" href="https://ko-fi.com/mkpoli" target="_blank" rel="noreferrer noopener">
				<span class="mark" aria-hidden="true">
					<svg viewBox="0 0 24 24"
						><path
							d="M4 8h12v5a5 5 0 0 1-5 5H9a5 5 0 0 1-5-5V8Zm12 1.5h1.5a2.5 2.5 0 0 1 0 5H16M7 3v2m3-2v2m3-2v2"
						/></svg
					>
				</span>
				<span class="copy"><strong>Ko-fi</strong><small>{text.kofi}</small></span>
				<svg class="arrow" viewBox="0 0 24 24" aria-hidden="true"
					><path d="M5 12h14m-6-6 6 6-6 6" /></svg
				>
			</a>
			<a
				class="company"
				href="mailto:mkpoli@mkpo.li?subject=Sponsorship%20on%20p.languagepatterns.org"
			>
				<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 6h16v12H4V6Zm0 1 8 6 8-6" /></svg>
				<span class="copy"><strong>{text.company}</strong><small>{text.companyHint}</small></span>
			</a>
			<p class="thanks">{text.thanks}</p>
		</div>
	</dialog>
{/if}

<style>
	.sponsor,
	.sponsor-dialog {
		--sp-ink: #c2185b;
		--sp-heart: #ff4f88;
		--sp-line: #ffc2d6;
		--sp-soft: #fff1f5;
		--sp-ring: #fff;
		--sp-surface: #fbf8f0;
		--sp-text: #2a2620;
		--sp-muted: #6b6357;
		--sp-border: #d8d2c4;
		--sp-hover: #efe9d8;
	}

	.sponsor {
		display: inline-flex;
		flex: none;
		align-items: center;
		gap: 0.55rem;
		min-height: 2.4rem;
		padding-block: 0;
		padding-inline: 0.35rem 1rem;
		border: 1px solid var(--sp-line);
		border-radius: 999px;
		background: linear-gradient(135deg, #ffe6ef, #fff0e0);
		color: var(--sp-ink);
		font: inherit;
		font-size: 1.02em;
		font-weight: bold;
		white-space: nowrap;
		cursor: pointer;
		box-shadow: 0 1px 2px rgb(194 24 91 / 0.12);
	}

	.sponsor:focus-visible,
	.dismiss:focus-visible,
	.kofi:focus-visible,
	.company:focus-visible {
		outline: 2px solid #8a5a14;
		outline-offset: 2px;
	}

	.face {
		position: relative;
		flex: none;
		width: 1.65rem;
		height: 1.65rem;
	}

	.face img {
		display: block;
		width: 100%;
		height: 100%;
		border-radius: 50%;
		object-fit: cover;
		box-shadow:
			0 0 0 1.5px var(--sp-ring),
			0 0 0 2.5px var(--sp-line);
	}

	.face-heart {
		position: absolute;
		inset-inline-end: -4px;
		bottom: -3px;
		width: 12px;
		height: 12px;
		fill: var(--sp-heart);
		stroke: var(--sp-ring);
		stroke-width: 2.5;
		paint-order: stroke;
		overflow: visible;
		transform-origin: 50% 60%;
	}

	@media (max-width: 720px) {
		.sponsor {
			width: 2.4rem;
			padding: 0;
			justify-content: center;
		}

		.label {
			display: none;
		}
	}

	.sponsor-dialog {
		width: min(480px, calc(100vw - 32px));
		max-height: calc(100dvh - 32px);
		padding: 0;
		border: 1px solid var(--sp-border);
		border-radius: 16px;
		background: var(--sp-surface);
		color: var(--sp-text);
		font-family: Georgia, Cambria, serif;
		line-height: 1.55;
		box-shadow: 0 30px 90px rgb(60 50 30 / 0.35);
		overflow: auto;
	}

	.sponsor-dialog::backdrop {
		background: rgb(0 0 0 / 0.4);
		backdrop-filter: blur(4px);
	}

	.top {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding-block: 14px 0;
		padding-inline: 24px 14px;
	}

	h2 {
		margin: 0;
		padding: 0;
		border: 0;
		font-size: 1.15rem;
		font-weight: bold;
		letter-spacing: normal;
	}

	.dismiss {
		display: grid;
		place-items: center;
		width: 36px;
		height: 36px;
		padding: 0;
		border: 0;
		border-radius: 50%;
		background: transparent;
		color: var(--sp-muted);
		cursor: pointer;
	}

	.dismiss svg {
		width: 20px;
		height: 20px;
	}

	.dismiss:hover {
		background: var(--sp-hover);
		color: var(--sp-text);
	}

	svg:not(.heart, .face-heart) {
		fill: none;
		stroke: currentColor;
		stroke-width: 2;
		stroke-linecap: round;
		stroke-linejoin: round;
	}

	.sheet {
		display: flex;
		flex-direction: column;
		gap: 14px;
		padding: 4px 24px 22px;
	}

	.sheet p {
		margin: 0;
	}

	.hero {
		position: relative;
		display: grid;
		place-items: center;
		height: 136px;
		background: radial-gradient(closest-side, #ffd9e5, transparent);
	}

	.portrait {
		position: relative;
		width: 112px;
		height: 112px;
	}

	.portrait img {
		display: block;
		width: 100%;
		height: 100%;
		border-radius: 50%;
		object-fit: cover;
		box-shadow:
			0 0 0 4px var(--sp-surface),
			0 0 0 6px var(--sp-line),
			0 10px 24px rgb(255 79 136 / 0.3);
	}

	.heart {
		position: absolute;
		inset-inline-end: -2px;
		bottom: 2px;
		width: 34px;
		height: 34px;
		fill: var(--sp-heart);
		stroke: var(--sp-surface);
		stroke-width: 2;
		paint-order: stroke;
		overflow: visible;
		transform-origin: 50% 60%;
		filter: drop-shadow(0 3px 6px rgb(255 79 136 / 0.35));
	}

	.heart .shine {
		fill: #fff;
		stroke: none;
		opacity: 0.6;
	}

	.spark {
		position: absolute;
		color: var(--sp-heart);
		opacity: 0.75;
	}

	.s1 {
		top: 22px;
		inset-inline-start: calc(50% - 84px);
		font-size: 14px;
	}

	.s2 {
		top: 12px;
		inset-inline-start: calc(50% + 66px);
		font-size: 10px;
	}

	.s3 {
		bottom: 20px;
		inset-inline-start: calc(50% - 76px);
		font-size: 11px;
	}

	.lead {
		font-size: 0.95rem;
		line-height: 1.65;
	}

	.ask {
		font-size: 1rem;
		font-weight: bold;
		line-height: 1.5;
		color: var(--sp-ink);
	}

	.how {
		padding: 12px 14px;
		border: 1px dashed var(--sp-line);
		border-radius: 14px;
		background: var(--sp-soft);
	}

	h3 {
		margin: 0 0 8px;
		font-size: 0.85rem;
		color: var(--sp-ink);
	}

	.steps {
		display: flex;
		flex-direction: column;
		gap: 6px;
		margin: 0;
		padding: 0;
		list-style: none;
		counter-reset: step;
	}

	.steps li {
		display: flex;
		align-items: baseline;
		gap: 8px;
		font-size: 0.9rem;
		line-height: 1.45;
		counter-increment: step;
	}

	.steps li::before {
		content: counter(step);
		display: grid;
		flex: none;
		place-items: center;
		width: 20px;
		height: 20px;
		border-radius: 50%;
		background: var(--sp-ink);
		color: var(--sp-soft);
		font-size: 11px;
		font-weight: bold;
		line-height: 1;
		transform: translateY(-1px);
	}

	.kofi {
		display: flex;
		align-items: center;
		gap: 12px;
		padding-block: 12px;
		padding-inline: 12px 18px;
		border-radius: 999px;
		background: linear-gradient(135deg, #d81b60, #b0124f);
		color: #fff;
		text-decoration: none;
		box-shadow: 0 6px 16px rgb(216 27 96 / 0.32);
	}

	.mark {
		display: grid;
		flex: none;
		place-items: center;
		width: 38px;
		height: 38px;
		border-radius: 50%;
		background: rgb(255 255 255 / 0.22);
	}

	.mark svg {
		width: 22px;
		height: 22px;
	}

	.arrow {
		flex: none;
		width: 18px;
		height: 18px;
	}

	.arrow:dir(rtl) {
		transform: scaleX(-1);
	}

	.copy {
		display: flex;
		flex: 1;
		flex-direction: column;
		gap: 1px;
		min-width: 0;
	}

	.copy strong {
		font-size: 0.9rem;
	}

	.copy small {
		font-size: 0.8rem;
		line-height: 1.4;
	}

	.kofi .copy strong {
		font-size: 1rem;
	}

	.company {
		display: flex;
		align-items: flex-start;
		gap: 10px;
		padding: 8px 12px;
		border-radius: 12px;
		color: var(--sp-muted);
		text-decoration: none;
	}

	.company:hover {
		background: var(--sp-hover);
		color: var(--sp-text);
	}

	.company > svg {
		flex: none;
		width: 18px;
		height: 18px;
		margin-top: 2px;
	}

	.company .copy strong {
		font-size: 0.85rem;
		color: var(--sp-text);
	}

	.thanks {
		font-size: 0.85rem;
		line-height: 1.5;
		color: var(--sp-muted);
		text-align: center;
	}

	@media (prefers-reduced-motion: no-preference) {
		.sponsor {
			transition:
				transform 180ms ease,
				box-shadow 180ms ease;
		}

		.sponsor:hover {
			transform: translateY(-1px);
			box-shadow: 0 4px 12px rgb(255 79 136 / 0.28);
		}

		.face-heart,
		.heart {
			animation: beat 2.8s ease-in-out infinite;
		}

		.sponsor:hover .face-heart,
		.sponsor:focus-visible .face-heart {
			animation: bounce 0.6s ease-in-out infinite;
		}

		.spark {
			animation: twinkle 2.4s ease-in-out infinite;
		}

		.s2 {
			animation-delay: 0.8s;
		}

		.s3 {
			animation-delay: 1.6s;
		}

		.sponsor-dialog[open] {
			animation: arrive 0.32s cubic-bezier(0.2, 0.8, 0.2, 1);
		}

		.kofi {
			transition:
				transform 180ms ease,
				box-shadow 180ms ease;
		}

		.kofi:hover {
			transform: translateY(-1px);
			box-shadow: 0 8px 22px rgb(216 27 96 / 0.42);
		}
	}

	@keyframes beat {
		0%,
		70%,
		100% {
			transform: scale(1);
		}
		78% {
			transform: scale(1.18);
		}
		86% {
			transform: scale(0.96);
		}
		93% {
			transform: scale(1.1);
		}
	}

	@keyframes bounce {
		0%,
		100% {
			transform: translateY(0) scale(1);
		}
		40% {
			transform: translateY(-2px) scale(1.18) rotate(-8deg);
		}
	}

	@keyframes twinkle {
		0%,
		100% {
			opacity: 0.2;
			transform: scale(0.7);
		}
		50% {
			opacity: 0.9;
			transform: scale(1);
		}
	}

	@keyframes arrive {
		from {
			opacity: 0;
			transform: translateY(14px) scale(0.98);
		}
	}
</style>
