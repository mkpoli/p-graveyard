<script lang="ts">
	type Status = 'alive' | 'dying' | 'dead' | 'reborn';

	type Evidence = {
		year: number;
		label: string;
		detail?: string;
	};

	type Branch = {
		/** Display form on this branch */
		form: string;
		/** Form after transition (only if transitions) */
		terminalForm?: string;
		/** Phonological environment that conditions this branch */
		condition?: string;
		/** Track index (0 is the trunk/main; higher = lower in row) */
		track: number;
		/** Optional parent track index — if set, a curved connector is drawn from parent at `splitAt` to this branch at `start` */
		parent?: number;
		/** Year this branch begins (split-off or first attestation) */
		start: number;
		/** Year this branch's parent stops carrying /p/ along this lineage; only meaningful with `parent` */
		splitAt?: number;
		/** Transition window: [a,b]. Inside the window the fill is a horizontal gradient warm→cool. */
		transition?: [number, number];
		/** Year this branch terminates. If `status === 'alive'`, use `END`. */
		end: number;
		/** Whether this branch still carries /p/, lost it, or was reborn from outside */
		status: Status;
		/** Whether the *start* of the branch is approximate (extrapolated) — drawn dashed */
		approxStart?: boolean;
		/** Whether the *end* of the branch is approximate — drawn dashed */
		approxEnd?: boolean;
		evidence?: Evidence[];
		note?: string;
	};

	type Language = {
		id: string;
		name: string;
		family: string;
		summary: string;
		description: string;
		branches: Branch[];
	};

	const START = -2000;
	const END = 2025;

	const languages: Language[] = [
		{
			id: 'celtic',
			name: 'Celtic',
			family: 'Indo-European → Celtic',
			summary: '*p > ∅',
			description:
				'Proto-Indo-European *p was lost very early in the Celtic branch — one of the most striking sound losses in IE. Latin pater corresponds to Old Irish athair "father", and Latin piscis to Old Irish íasc "fish".',
			branches: [
				{
					form: '*p',
					terminalForm: '∅',
					track: 0,
					start: -2000,
					transition: [-1500, -800],
					end: 2025,
					status: 'dead',
					approxStart: true,
					evidence: [
						{ year: -2000, label: 'PIE *ph₂tḗr', detail: 'reconstructed' },
						{ year: -500, label: 'Gaulish atir', detail: '/p/ already gone' },
						{ year: 800, label: 'Old Irish athair', detail: 'no /p/' }
					]
				}
			]
		},
		{
			id: 'arabic',
			name: 'Arabic',
			family: 'Afro-Asiatic → Semitic',
			summary: '*p > /f/',
			description:
				'Proto-Semitic *p merged into /f/ on the way to Arabic. Sister languages preserve traces — Hebrew has [p]~[f] allophony — but Arabic has only /f/ in inherited words, and borrows /p/ as /b/ or /f/.',
			branches: [
				{
					form: '*p',
					terminalForm: '/f/',
					track: 0,
					start: -2000,
					transition: [-1500, -500],
					end: 2025,
					status: 'dead',
					approxStart: true,
					evidence: [
						{ year: -2000, label: 'Proto-Semitic *p', detail: 'reconstructed' },
						{ year: -200, label: 'Old Arabic inscriptions', detail: 'no /p/ orthographically' },
						{ year: 700, label: 'Classical Arabic /f/', detail: 'fully merged' }
					]
				}
			]
		},
		{
			id: 'chinese',
			name: 'Chinese',
			family: 'Sino-Tibetan',
			summary: '*p > /p/ (heavy lab.); *p > /f/ (light lab.)',
			description:
				'In late Middle Chinese, the labial initials split (輕唇音 vs 重唇音): /p, pʰ, b/ became /f, fʰ, v/ before specific medial-vowel combinations, while the rest preserved /p/. Mandarin still has plenty of /p/, but a large vocabulary "leaked" out into /f/.',
			branches: [
				{
					form: 'OC *p',
					track: 0,
					start: -1200,
					end: 900,
					status: 'alive',
					approxStart: true,
					evidence: [
						{ year: -1000, label: 'Old Chinese *p-', detail: 'reconstructed' },
						{ year: 600, label: 'Qieyun 切韻', detail: 'still /p/ in all environments' }
					]
				},
				{
					form: 'MC /p/',
					track: 0,
					parent: 0,
					splitAt: 900,
					start: 900,
					end: 2025,
					status: 'alive',
					note: 'heavy labial 重唇音 — /p/ kept',
					evidence: [{ year: 1700, label: '北 /pei̯/', detail: '/p/ preserved' }]
				},
				{
					form: 'MC /p/',
					terminalForm: '/f/',
					condition: '_jɨ, _ju, _jo',
					track: 1,
					parent: 0,
					splitAt: 900,
					start: 900,
					transition: [900, 1300],
					end: 2025,
					status: 'dead',
					note: 'light labial 輕唇音 — labiodentalization',
					evidence: [
						{ year: 1100, label: '韻鏡', detail: '輕唇音 split codified' },
						{ year: 1324, label: '中原音韻', detail: '/f/ established' },
						{ year: 2025, label: '飛 /feɪ̯/', detail: 'modern /f/ < MC *pjɨj' }
					]
				}
			]
		},
		{
			id: 'japanese',
			name: 'Japanese',
			family: 'Japonic',
			summary: '*p > /ɸ/ > /h, ç, ɸ, p/',
			description:
				'Old Japanese /p/ weakened to /ɸ/ by the late Heian period, then split by following vowel. Inherited /p/ survives only after geminates and moraic nasals (e.g. 切符 kippu, 散歩 sanpo). /p/ then re-entered the language wholesale through Portuguese and modern loanwords.',
			branches: [
				{
					form: 'OJ /p/',
					track: 0,
					start: 600,
					end: 800,
					status: 'alive',
					approxStart: true,
					evidence: [{ year: 720, label: "Man'yōgana", detail: 'OJ /p/ as [p]' }]
				},
				{
					form: '/p/',
					condition: '{Q,N}_V',
					track: 0,
					parent: 0,
					splitAt: 800,
					start: 800,
					end: 2025,
					status: 'alive',
					note: 'preserved after geminate or nasal',
					evidence: [{ year: 2025, label: 'kippu, sanpo', detail: '/p/ kept' }]
				},
				{
					form: '/ɸ/',
					terminalForm: '/h/',
					condition: '_{a,o,e}',
					track: 1,
					parent: 0,
					splitAt: 800,
					start: 800,
					transition: [1400, 1700],
					end: 2025,
					status: 'dead',
					evidence: [
						{ year: 1100, label: 'Heian texts', detail: '/ɸ/ in all V_V positions' },
						{ year: 1603, label: 'Nippo Jisho', detail: '"f" still attested' },
						{ year: 1900, label: 'mJ /h/', detail: 'fully /h/ before /a, e, o/' }
					]
				},
				{
					form: '/ɸ/',
					terminalForm: '/ç/',
					condition: '_i',
					track: 2,
					parent: 0,
					splitAt: 800,
					start: 800,
					transition: [1400, 1700],
					end: 2025,
					status: 'dead'
				},
				{
					form: '/ɸ/',
					condition: '_u',
					track: 3,
					parent: 0,
					splitAt: 800,
					start: 800,
					end: 2025,
					status: 'dying',
					note: '/ɸu/ holds out, marginal'
				},
				{
					form: '/p/',
					condition: 'loans',
					track: 4,
					start: 1543,
					end: 2025,
					status: 'reborn',
					note: 'Portuguese, then Dutch, English…',
					evidence: [
						{ year: 1543, label: 'Portuguese arrives', detail: 'pan, tabako' },
						{ year: 1900, label: 'English loans', detail: 'pen, computer' }
					]
				}
			]
		}
	];

	// ─── Layout constants
	const labelWidth = 170;
	const plotWidth = 880;
	const trackHeight = 26;
	const trackGap = 8;
	const rowPad = 14;
	const headerHeight = 50;
	const footerHeight = 30;

	const yearSpan = END - START;

	function x(year: number): number {
		return labelWidth + ((year - START) / yearSpan) * plotWidth;
	}

	function trackY(rowTop: number, track: number): number {
		return rowTop + rowPad + track * (trackHeight + trackGap) + trackHeight / 2;
	}

	const rowOffsets = $derived.by(() => {
		const offsets: number[] = [];
		let y = headerHeight;
		for (const lang of languages) {
			offsets.push(y);
			const trackCount = Math.max(...lang.branches.map((b) => b.track)) + 1;
			y += rowPad * 2 + trackCount * (trackHeight + trackGap) - trackGap;
		}
		return offsets;
	});

	const totalHeight = $derived.by(() => {
		const last = rowOffsets.at(-1) ?? headerHeight;
		const lastLang = languages.at(-1)!;
		const trackCount = Math.max(...lastLang.branches.map((b) => b.track)) + 1;
		const rowH = rowPad * 2 + trackCount * (trackHeight + trackGap) - trackGap;
		return last + rowH + footerHeight;
	});

	const totalWidth = labelWidth + plotWidth + 16;

	const ticks = [-2000, -1500, -1000, -500, 0, 500, 1000, 1500, 2000];

	function fmtYear(y: number): string {
		if (y === 0) return '0';
		return y < 0 ? `${-y} BCE` : `${y} CE`;
	}

	let hovered: { branch: Branch; lang: Language } | null = $state(null);
	let hoveredEvidence: Evidence | null = $state(null);
</script>

<figure>
	<div class="scroll">
		<svg
			viewBox={`0 0 ${totalWidth} ${totalHeight}`}
			width={totalWidth}
			role="img"
			aria-label="Historical timeline of /p/ loss across languages"
		>
			<defs>
				<linearGradient id="trans" x1="0" x2="1" y1="0" y2="0">
					<stop offset="0%" stop-color="var(--p-alive)" />
					<stop offset="100%" stop-color="var(--p-dead)" />
				</linearGradient>
				<pattern
					id="extrapolated"
					patternUnits="userSpaceOnUse"
					width="8"
					height="8"
					patternTransform="rotate(45)"
				>
					<rect width="8" height="8" fill="var(--bg-soft)" />
					<rect width="2" height="8" fill="var(--p-alive)" opacity="0.4" />
				</pattern>
			</defs>

			<!-- year ticks -->
			<g class="axis">
				{#each ticks as t (t)}
					<line x1={x(t)} x2={x(t)} y1={headerHeight - 8} y2={totalHeight - footerHeight + 4} />
					<text x={x(t)} y={headerHeight - 16} text-anchor="middle">{fmtYear(t)}</text>
				{/each}
				<line
					x1={labelWidth}
					x2={labelWidth + plotWidth}
					y1={headerHeight - 8}
					y2={headerHeight - 8}
					class="axis-baseline"
				/>
			</g>

			{#each languages as lang, li (lang.id)}
				{@const yTop = rowOffsets[li]}
				{@const trackCount = Math.max(...lang.branches.map((b) => b.track)) + 1}
				{@const rowH = rowPad * 2 + trackCount * (trackHeight + trackGap) - trackGap}

				<!-- row separator -->
				{#if li > 0}
					<line
						x1={0}
						x2={totalWidth}
						y1={yTop}
						y2={yTop}
						stroke="var(--rule)"
						stroke-width="1"
					/>
				{/if}

				<!-- language label -->
				<g class="label">
					<text x={14} y={yTop + 22} class="lang-name">{lang.name}</text>
					<text x={14} y={yTop + 38} class="lang-family">{lang.family}</text>
					<text x={14} y={yTop + 54} class="lang-summary">{lang.summary}</text>
				</g>

				<!-- branch connectors (curves from parent split point) -->
				{#each lang.branches as b (b.track + '-' + b.start)}
					{#if b.parent !== undefined && b.splitAt !== undefined}
						{@const px = x(b.splitAt)}
						{@const py = trackY(yTop, b.parent)}
						{@const cy = trackY(yTop, b.track)}
						{@const cx = px}
						<path
							class="connector"
							d={`M ${px} ${py} C ${cx + 30} ${py} ${cx} ${cy} ${cx + 30} ${cy}`}
							fill="none"
						/>
					{/if}
				{/each}

				<!-- branch bands -->
				{#each lang.branches as b (b.track + '-' + b.start)}
					{@const by = trackY(yTop, b.track) - trackHeight / 2}
					{@const segStart = b.parent !== undefined ? Math.max(b.start, b.splitAt ?? b.start) : b.start}
					{@const aliveEnd = b.transition ? b.transition[0] : b.end}
					{@const deadStart = b.transition ? b.transition[1] : b.end}

					<!-- alive segment (warm) -->
					{#if aliveEnd > segStart}
						<rect
							x={x(segStart)}
							y={by}
							width={Math.max(2, x(aliveEnd) - x(segStart))}
							height={trackHeight}
							rx="4"
							class="band-alive"
							class:approx={b.approxStart && segStart === b.start}
							onmouseenter={() => (hovered = { branch: b, lang })}
							onmouseleave={() => (hovered = null)}
							role="img"
							aria-label={`${lang.name} ${b.form}`}
						/>
					{/if}

					<!-- transition gradient -->
					{#if b.transition}
						<rect
							x={x(b.transition[0])}
							y={by}
							width={Math.max(2, x(b.transition[1]) - x(b.transition[0]))}
							height={trackHeight}
							rx="4"
							fill="url(#trans)"
							class="band-trans"
							role="img"
							aria-label={`${lang.name} ${b.form} transitioning to ${b.terminalForm ?? '∅'}`}
							onmouseenter={() => (hovered = { branch: b, lang })}
							onmouseleave={() => (hovered = null)}
						/>
					{/if}

					<!-- dead/post segment (cool) -->
					{#if b.transition && b.end > deadStart}
						<rect
							x={x(deadStart)}
							y={by}
							width={Math.max(2, x(b.end) - x(deadStart))}
							height={trackHeight}
							rx="4"
							class="band-dead"
							class:approx={b.approxEnd}
							role="img"
							aria-label={`${lang.name} ${b.terminalForm ?? 'lost'}`}
							onmouseenter={() => (hovered = { branch: b, lang })}
							onmouseleave={() => (hovered = null)}
						/>
					{/if}

					<!-- form label inside band -->
					<text
						x={x(segStart) + 8}
						y={by + trackHeight / 2 + 4}
						class="band-form"
						pointer-events="none">{b.form}</text
					>

					{#if b.terminalForm}
						<text
							x={x(b.end) - 8}
							y={by + trackHeight / 2 + 4}
							class="band-form band-form-end"
							text-anchor="end"
							pointer-events="none">→ {b.terminalForm}</text
						>
					{/if}

					<!-- end marker -->
					{#if b.status === 'alive' || b.status === 'reborn'}
						<text
							x={x(b.end) + 4}
							y={by + trackHeight / 2 + 5}
							class="end-marker alive"
							pointer-events="none">●</text
						>
					{:else if b.status === 'dying'}
						<text
							x={x(b.end) + 4}
							y={by + trackHeight / 2 + 5}
							class="end-marker dying"
							pointer-events="none">◐</text
						>
					{:else}
						<text
							x={x(b.end) + 4}
							y={by + trackHeight / 2 + 5}
							class="end-marker dead"
							pointer-events="none">✕</text
						>
					{/if}

					<!-- condition tag -->
					{#if b.condition}
						<text x={x(segStart) + 8} y={by - 4} class="cond" pointer-events="none"
							>/{b.condition}/</text
						>
					{/if}

					<!-- evidence dots -->
					{#if b.evidence}
						{#each b.evidence as ev (ev.year + ev.label)}
							<g
								class="ev"
								onmouseenter={() => (hoveredEvidence = ev)}
								onmouseleave={() => (hoveredEvidence = null)}
								role="button"
								tabindex="0"
							>
								<circle cx={x(ev.year)} cy={by + trackHeight / 2} r="4" />
								<title>{fmtYear(ev.year)} — {ev.label}{ev.detail
										? ` (${ev.detail})`
										: ''}</title>
							</g>
						{/each}
					{/if}
				{/each}
			{/each}
		</svg>
	</div>

	{#if hovered}
		<div class="tip">
			<strong>{hovered.lang.name}</strong> · <span class="mono">{hovered.branch.form}</span>
			{#if hovered.branch.terminalForm}
				<span class="mono"> → {hovered.branch.terminalForm}</span>
			{/if}
			{#if hovered.branch.condition}<span class="cond-inline"
					>/ {hovered.branch.condition} /</span
				>{/if}
			{#if hovered.branch.note}<div class="tip-note">{hovered.branch.note}</div>{/if}
		</div>
	{:else if hoveredEvidence}
		<div class="tip">
			<strong>{fmtYear(hoveredEvidence.year)}</strong> — {hoveredEvidence.label}
			{#if hoveredEvidence.detail}<div class="tip-note">{hoveredEvidence.detail}</div>{/if}
		</div>
	{/if}

	<figcaption>
		<ul class="legend">
			<li><span class="sw sw-alive"></span> /p/ retained</li>
			<li><span class="sw sw-trans"></span> transition (interpolated)</li>
			<li><span class="sw sw-dead"></span> /p/ lost</li>
			<li><span class="marker alive">●</span> still attested</li>
			<li><span class="marker dying">◐</span> marginal/dying</li>
			<li><span class="marker dead">✕</span> lost</li>
			<li><span class="ev-dot"></span> evidence point</li>
		</ul>
	</figcaption>
</figure>

<style>
	figure {
		--p-alive: #c08a3e;
		--p-alive-soft: #e8d4a8;
		--p-dead: #5a6470;
		--p-dead-soft: #c2c8d0;
		--rule: #d8d2c4;
		--bg-soft: #f5f0e4;
		--ink: #2a2620;
		--ink-soft: #6b6357;

		margin: 0;
		font-family: system-ui, sans-serif;
	}

	.scroll {
		overflow-x: auto;
		background: #fbf8f0;
		border: 1px solid var(--rule);
		border-radius: 8px;
		padding: 8px;
	}

	svg {
		display: block;
		min-width: 100%;
		height: auto;
	}

	.axis line {
		stroke: #e6dfd0;
		stroke-width: 1;
	}
	.axis-baseline {
		stroke: var(--rule) !important;
	}
	.axis text {
		font-size: 11px;
		fill: var(--ink-soft);
		font-family: ui-monospace, monospace;
	}

	.label .lang-name {
		font-size: 14px;
		font-weight: 600;
		fill: var(--ink);
	}
	.label .lang-family {
		font-size: 10px;
		fill: var(--ink-soft);
	}
	.label .lang-summary {
		font-size: 11px;
		fill: var(--ink-soft);
		font-family: ui-monospace, monospace;
	}

	.band-alive {
		fill: var(--p-alive);
		opacity: 0.9;
	}
	.band-dead {
		fill: var(--p-dead);
		opacity: 0.85;
	}
	.band-trans {
		opacity: 0.95;
	}
	.band-alive.approx,
	.band-dead.approx {
		fill: url(#extrapolated);
		opacity: 0.6;
	}

	.band-form {
		font-size: 11px;
		font-family: ui-monospace, monospace;
		fill: #fff;
		font-weight: 600;
	}
	.band-form-end {
		fill: #fff;
	}

	.cond {
		font-size: 9px;
		fill: var(--ink-soft);
		font-family: ui-monospace, monospace;
		font-style: italic;
	}

	.connector {
		stroke: var(--p-alive);
		stroke-width: 1.5;
		opacity: 0.55;
	}

	.end-marker {
		font-size: 14px;
		font-family: ui-monospace, monospace;
	}
	.end-marker.alive {
		fill: #2d7a3e;
	}
	.end-marker.dying {
		fill: #b88a2e;
	}
	.end-marker.dead {
		fill: #8a3d3d;
	}

	.ev circle {
		fill: var(--ink);
		stroke: #fff;
		stroke-width: 1.5;
		cursor: help;
	}
	.ev:hover circle {
		fill: #d4a64a;
	}

	.tip {
		margin-top: 8px;
		padding: 8px 12px;
		background: #fbf8f0;
		border: 1px solid var(--rule);
		border-radius: 6px;
		font-size: 13px;
		color: var(--ink);
	}
	.tip .mono {
		font-family: ui-monospace, monospace;
	}
	.tip-note {
		margin-top: 4px;
		color: var(--ink-soft);
		font-size: 12px;
	}
	.cond-inline {
		font-family: ui-monospace, monospace;
		color: var(--ink-soft);
		margin-left: 6px;
		font-size: 12px;
	}

	figcaption {
		margin-top: 10px;
	}
	.legend {
		list-style: none;
		padding: 0;
		margin: 0;
		display: flex;
		flex-wrap: wrap;
		gap: 6px 16px;
		font-size: 12px;
		color: var(--ink-soft);
	}
	.legend li {
		display: flex;
		align-items: center;
		gap: 6px;
	}
	.sw {
		display: inline-block;
		width: 22px;
		height: 10px;
		border-radius: 2px;
	}
	.sw-alive {
		background: var(--p-alive);
	}
	.sw-dead {
		background: var(--p-dead);
	}
	.sw-trans {
		background: linear-gradient(to right, var(--p-alive), var(--p-dead));
	}
	.marker {
		font-family: ui-monospace, monospace;
		font-size: 14px;
	}
	.marker.alive {
		color: #2d7a3e;
	}
	.marker.dying {
		color: #b88a2e;
	}
	.marker.dead {
		color: #8a3d3d;
	}
	.ev-dot {
		display: inline-block;
		width: 8px;
		height: 8px;
		border-radius: 50%;
		background: var(--ink);
		border: 1.5px solid #fff;
		box-shadow: 0 0 0 1px var(--ink);
	}
</style>
