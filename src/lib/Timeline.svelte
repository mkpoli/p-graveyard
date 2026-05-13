<script lang="ts">
	type Evidence = { year: number; label: string; detail?: string };

	type Stop = {
		year: number;
		phoneme: string;
		/** 0..1; controls opacity. Used to fade pre-evidence sections in. */
		opacity?: number;
	};

	type Flow = {
		id: string;
		family: 'pie' | 'psem' | 'oc' | 'pj' | 'drav';
		label: string;
		/** Sub-label for tooltip (e.g. conditioning environment) */
		condition?: string;
		/** Lane within the family — fractional values allowed; controls vertical position */
		lane: number;
		/** Parent flow id and the year at which it splits off */
		parent?: { id: string; year: number };
		/** First year this flow renders. */
		start: number;
		/** Last year this flow extends to. */
		end: number;
		/** Year before `start` from which the flow fades in (extrapolation). */
		fadeFrom?: number;
		/** Color stops for the band. Color is interpolated across years. */
		stops: Stop[];
		evidence?: Evidence[];
		status: 'alive' | 'dead' | 'dying' | 'borrowed';
		/** Classification of how /p/ developed in this lineage */
		pattern?: Pattern;
		note?: string;
	};

	// Perceptual palette: /p/ at the warm center. Bilabial/labial sounds
	// (ɸ, β, b, pʰ) stay in the warm family — small hue shifts from amber.
	// Far sounds (h glottal, ç palatal) move to cool greens/teals. ∅ is the
	// same hue as /p/, just desaturated and lightened — a natural "fade-out"
	// rather than a different sound.
	const PHONEME_COLOR: Record<string, string> = {
		p: '#c89240',
		'*p': '#c89240',
		'pʰ': '#d8a04b',
		b: '#9a6e30',
		f: '#b58850',
		'*f': '#b58850',
		ɸ: '#bf7a52',
		β: '#b06e5e',
		v: '#8d6e3e',
		h: '#4f9166',
		ç: '#2f7e85',
		w: '#c8836a',
		'∅': '#d8c8a8'
	};

	type Pattern =
		| 'preserved'
		| 'fricativization'
		| 'weakening'
		| 'loss'
		| 'restricted'
		| 'introduced';

	const PATTERN_INFO: Record<Pattern, { glyph: string; label: string; tip: string }> = {
		preserved: { glyph: '=', label: 'preserved', tip: '/p/ kept as [p]' },
		fricativization: { glyph: 'ƒ', label: 'fricativization', tip: '/p/ → fricative (/f/, /ɸ/)' },
		weakening: {
			glyph: 'h',
			label: 'weakening / debuccalization',
			tip: '/p/ → /ɸ/ → /h/ (oral place dissolves)'
		},
		loss: { glyph: '∅', label: 'total loss', tip: '/p/ deleted from the inventory' },
		restricted: {
			glyph: '◇',
			label: 'restricted survival',
			tip: '/p/ kept only in specific environments'
		},
		introduced: {
			glyph: '+',
			label: 'introduced (loans)',
			tip: '/p/ enters via borrowing in new positions'
		}
	};

	const families: { id: Flow['family']; name: string; sub: string }[] = [
		{ id: 'pie', name: 'Indo-European', sub: 'PIE *p → daughters' },
		{ id: 'psem', name: 'Semitic', sub: 'Proto-Semitic *p → daughters' },
		{ id: 'drav', name: 'Dravidian', sub: 'PSD *p → daughters' },
		{ id: 'oc', name: 'Sinitic', sub: 'Old Chinese *p → Mandarin' },
		{ id: 'pj', name: 'Japonic', sub: 'Proto-Japonic *p → Japanese' }
	];

	const flows: Flow[] = [
		// ─── Indo-European
		{
			id: 'pie',
			family: 'pie',
			label: 'PIE *p',
			lane: 0,
			start: -3500,
			end: -2500,
			fadeFrom: -5000,
			stops: [
				{ year: -5000, phoneme: 'p', opacity: 0 },
				{ year: -3500, phoneme: 'p', opacity: 1 },
				{ year: -2500, phoneme: 'p', opacity: 1 }
			],
			evidence: [{ year: -3500, label: 'PIE *ph₂tḗr', detail: 'reconstructed' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'celtic',
			family: 'pie',
			label: 'Celtic',
			condition: '*p > ∅',
			parent: { id: 'pie', year: -2500 },
			lane: -2,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: -1500, phoneme: 'p' },
				{ year: -800, phoneme: '∅' },
				{ year: 2025, phoneme: '∅' }
			],
			evidence: [
				{ year: -500, label: 'Gaulish atir', detail: '/p/ already gone' },
				{ year: 800, label: 'OIr. athair "father"', detail: 'cf. Lat. pater' }
			],
			status: 'dead',
			pattern: 'loss',
			note: 'PIE *p disappeared early in Proto-Celtic. May have passed through *ɸ > *h before vanishing.'
		},
		{
			id: 'germanic',
			family: 'pie',
			label: 'Germanic',
			condition: '*p > *f (Grimm)',
			parent: { id: 'pie', year: -2500 },
			lane: -1,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: -500, phoneme: 'p' },
				{ year: 100, phoneme: 'f' },
				{ year: 2025, phoneme: 'f' }
			],
			evidence: [
				{ year: -200, label: "Grimm's Law", detail: 'PIE *p → PGmc *f' },
				{ year: 800, label: 'OE fæder', detail: '< PGmc *fader' }
			],
			status: 'dead',
			pattern: 'fricativization',
			note: 'Inherited *p turned to *f via Grimm. Modern /p/ in Germanic comes mostly from loans and other sources.'
		},
		{
			id: 'italic',
			family: 'pie',
			label: 'Italic / Romance',
			parent: { id: 'pie', year: -2500 },
			lane: 0,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [
				{ year: -700, label: 'Old Latin', detail: '/p/ preserved' },
				{ year: 1300, label: 'Romance daughters', detail: '/p/ kept across the board' }
			],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'hellenic',
			family: 'pie',
			label: 'Greek',
			parent: { id: 'pie', year: -2500 },
			lane: 1,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [
				{ year: -1450, label: 'Linear B pa-te-re', detail: 'Mycenaean' },
				{ year: -700, label: 'Homeric πατήρ', detail: '' }
			],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'iir',
			family: 'pie',
			label: 'Indo-Iranian',
			parent: { id: 'pie', year: -2500 },
			lane: 2,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [{ year: -1500, label: 'Vedic pitár', detail: '' }],
			status: 'alive',
			pattern: 'preserved'
		},

		// ─── Semitic
		{
			id: 'psem',
			family: 'psem',
			label: 'Proto-Semitic *p',
			lane: 0,
			start: -3500,
			end: -2500,
			fadeFrom: -5000,
			stops: [
				{ year: -5000, phoneme: 'p', opacity: 0 },
				{ year: -3500, phoneme: 'p', opacity: 1 },
				{ year: -2500, phoneme: 'p', opacity: 1 }
			],
			evidence: [{ year: -3500, label: 'PSem *p', detail: 'reconstructed' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'arabic',
			family: 'psem',
			label: 'Arabic',
			condition: '*p > /f/',
			parent: { id: 'psem', year: -2500 },
			lane: -1,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: -1500, phoneme: 'p' },
				{ year: -200, phoneme: 'f' },
				{ year: 2025, phoneme: 'f' }
			],
			evidence: [
				{ year: -200, label: 'Old Arabic inscr.', detail: '' },
				{ year: 700, label: 'Classical Arabic', detail: 'no /p/ in inherited words' }
			],
			status: 'dead',
			pattern: 'fricativization',
			note: 'PSem *pūm "mouth" → Arabic fam-. Loan /p/ adapted as /b/ or written with پ.'
		},
		{
			id: 'hebrew',
			family: 'psem',
			label: 'Hebrew',
			parent: { id: 'psem', year: -2500 },
			lane: 0,
			start: -2500,
			end: 2025,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [{ year: -800, label: 'Biblical Hebrew', detail: '[p]~[f] allophony' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'akkadian',
			family: 'psem',
			label: 'Akkadian',
			parent: { id: 'psem', year: -2500 },
			lane: 1,
			start: -2500,
			end: 100,
			stops: [
				{ year: -2500, phoneme: 'p' },
				{ year: 100, phoneme: 'p' }
			],
			evidence: [{ year: -2300, label: 'Old Akkadian', detail: '/p/ preserved' }],
			status: 'alive',
			pattern: 'preserved',
			note: 'Language died ~100 CE but /p/ was preserved throughout its history.'
		},

		// ─── Dravidian
		{
			id: 'psd',
			family: 'drav',
			label: 'Proto-South-Dravidian *p',
			lane: 0,
			start: -2000,
			end: 0,
			fadeFrom: -3500,
			stops: [
				{ year: -3500, phoneme: 'p', opacity: 0 },
				{ year: -2000, phoneme: 'p', opacity: 1 },
				{ year: 0, phoneme: 'p', opacity: 1 }
			],
			evidence: [{ year: -2000, label: 'PSD *p', detail: 'reconstructed' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'tamil',
			family: 'drav',
			label: 'Tamil',
			parent: { id: 'psd', year: 0 },
			lane: -1,
			start: 0,
			end: 2025,
			stops: [
				{ year: 0, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [
				{ year: 300, label: 'Tolkāppiyam', detail: '/p/ preserved' },
				{ year: 2025, label: 'Tamil pāl "milk"', detail: '' }
			],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'kannada',
			family: 'drav',
			label: 'Kannada',
			condition: '#_ — /p/ > /h/ (debuccalization)',
			parent: { id: 'psd', year: 0 },
			lane: 1,
			start: 0,
			end: 2025,
			stops: [
				{ year: 0, phoneme: 'p' },
				{ year: 1000, phoneme: 'p' },
				{ year: 1400, phoneme: 'h' },
				{ year: 2025, phoneme: 'h' }
			],
			evidence: [
				{ year: 500, label: 'Old Kannada pāl', detail: '/p/ preserved' },
				{ year: 1500, label: 'Middle Kannada hāl(u)', detail: 'initial /p/ → /h/' },
				{ year: 2025, label: 'Modern hāl(u) "milk"', detail: '' }
			],
			status: 'dead',
			pattern: 'weakening',
			note: 'South Dravidian *p > /h/ at word-initial position in Middle Kannada. /p/ survives elsewhere and in loans.'
		},

		// ─── Sinitic
		{
			id: 'oc',
			family: 'oc',
			label: 'Old Chinese *p',
			lane: 0,
			start: -1200,
			end: 900,
			fadeFrom: -2500,
			stops: [
				{ year: -2500, phoneme: 'p', opacity: 0 },
				{ year: -1200, phoneme: 'p', opacity: 1 },
				{ year: 900, phoneme: 'p', opacity: 1 }
			],
			evidence: [
				{ year: -1000, label: 'OBI / Bronzeware', detail: 'OC *p reconstructed' },
				{ year: 600, label: 'Qieyun 切韻', detail: '/p/ in all environments' }
			],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'oc-heavy',
			family: 'oc',
			label: '重唇音',
			condition: 'elsewhere — /p/ kept',
			parent: { id: 'oc', year: 900 },
			lane: -1,
			start: 900,
			end: 2025,
			stops: [
				{ year: 900, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [{ year: 2025, label: '北 běi', detail: '/p/ preserved' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'oc-light',
			family: 'oc',
			label: '輕唇音',
			condition: '_jɨ, _ju, _jo — /p/ > /f/',
			parent: { id: 'oc', year: 900 },
			lane: 1,
			start: 900,
			end: 2025,
			stops: [
				{ year: 900, phoneme: 'p' },
				{ year: 1300, phoneme: 'f' },
				{ year: 2025, phoneme: 'f' }
			],
			evidence: [
				{ year: 1100, label: '韻鏡 Yùnjìng', detail: '輕唇音 split codified' },
				{ year: 1324, label: '中原音韻', detail: '/f/ established' },
				{ year: 2025, label: '飛 fēi, 風 fēng', detail: 'modern /f/ < MC *pjV' }
			],
			status: 'dead',
			pattern: 'fricativization',
			note: 'Late Middle Chinese labiodentalization.'
		},

		// ─── Japonic
		{
			id: 'pj',
			family: 'pj',
			label: 'Proto-Japonic *p',
			lane: 0,
			start: -300,
			end: 600,
			fadeFrom: -1500,
			stops: [
				{ year: -1500, phoneme: 'p', opacity: 0 },
				{ year: -300, phoneme: 'p', opacity: 1 },
				{ year: 600, phoneme: 'p', opacity: 1 }
			],
			evidence: [{ year: -300, label: 'PJ *p', detail: 'reconstructed' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'oj',
			family: 'pj',
			label: 'Old Japanese',
			parent: { id: 'pj', year: 600 },
			lane: 0,
			start: 600,
			end: 800,
			stops: [
				{ year: 600, phoneme: 'p' },
				{ year: 800, phoneme: 'p' }
			],
			evidence: [{ year: 720, label: "Man'yōgana", detail: '/p/ likely [p]' }],
			status: 'alive',
			pattern: 'preserved'
		},
		{
			id: 'pj-qn',
			family: 'pj',
			label: '{Q,N}_V',
			condition: 'after geminate / nasal — /p/ kept',
			parent: { id: 'oj', year: 800 },
			lane: -3,
			start: 800,
			end: 2025,
			stops: [
				{ year: 800, phoneme: 'p' },
				{ year: 2025, phoneme: 'p' }
			],
			evidence: [{ year: 2025, label: '切符, 散歩', detail: 'kippu, sanpo' }],
			status: 'alive',
			pattern: 'restricted'
		},
		{
			id: 'pj-aoe',
			family: 'pj',
			label: '#_{a,o,e}',
			condition: 'word-initial → /ɸ/ → /h/',
			parent: { id: 'oj', year: 800 },
			lane: -2,
			start: 800,
			end: 2025,
			stops: [
				{ year: 800, phoneme: 'ɸ' },
				{ year: 1400, phoneme: 'ɸ' },
				{ year: 1700, phoneme: 'h' },
				{ year: 2025, phoneme: 'h' }
			],
			evidence: [
				{ year: 1100, label: 'Heian /ɸ/', detail: '' },
				{ year: 1603, label: 'Nippo Jisho "f"', detail: '/ɸ/ still attested' },
				{ year: 1900, label: 'modern /h/', detail: 'hana, hi, hone' }
			],
			status: 'dead',
			pattern: 'weakening'
		},
		{
			id: 'pj-i',
			family: 'pj',
			label: '#_i',
			condition: 'word-initial → /ɸ/ → /ç/',
			parent: { id: 'oj', year: 800 },
			lane: -1,
			start: 800,
			end: 2025,
			stops: [
				{ year: 800, phoneme: 'ɸ' },
				{ year: 1700, phoneme: 'ɸ' },
				{ year: 1900, phoneme: 'ç' },
				{ year: 2025, phoneme: 'ç' }
			],
			evidence: [{ year: 2025, label: 'ひと hito [çito]', detail: '' }],
			status: 'dead',
			pattern: 'weakening'
		},
		{
			id: 'pj-u',
			family: 'pj',
			label: '#_u',
			condition: 'word-initial → /ɸ/ (kept marginally)',
			parent: { id: 'oj', year: 800 },
			lane: 0,
			start: 800,
			end: 2025,
			stops: [
				{ year: 800, phoneme: 'ɸ' },
				{ year: 2025, phoneme: 'ɸ' }
			],
			evidence: [{ year: 2025, label: 'ふた futa [ɸɯta]', detail: '' }],
			status: 'dying',
			pattern: 'weakening'
		},
		{
			id: 'pj-vv',
			family: 'pj',
			label: 'V_V',
			condition: 'intervocalic — /ɸ/ → /β/ → /w/ → ∅',
			parent: { id: 'oj', year: 800 },
			lane: 1,
			start: 800,
			end: 2025,
			stops: [
				{ year: 800, phoneme: 'ɸ' },
				{ year: 1100, phoneme: 'ɸ' },
				{ year: 1300, phoneme: 'β' },
				{ year: 1500, phoneme: 'w' },
				{ year: 1700, phoneme: 'w' },
				{ year: 1900, phoneme: '∅' },
				{ year: 2025, phoneme: '∅' }
			],
			evidence: [
				{ year: 900, label: 'kapa "river"', detail: 'OJ [p]' },
				{ year: 1300, label: 'kaβa', detail: 'medial /p/ > /β/' },
				{ year: 1700, label: 'kawa', detail: '/β/ > /w/' },
				{ year: 2025, label: 'omou < omopu', detail: '/w/ deleted before non-/a/' }
			],
			status: 'dead',
			pattern: 'loss',
			note: 'Medial /p/ went through /ɸ/ → /β/ → /w/ and then deleted in many environments.'
		},
		{
			id: 'pj-loan',
			family: 'pj',
			label: 'Loanwords',
			condition: 'Portuguese, Dutch, English — /p/ in new positions via loans',
			lane: 2,
			start: 1543,
			end: 2025,
			fadeFrom: 1400,
			stops: [
				{ year: 1400, phoneme: 'p', opacity: 0 },
				{ year: 1543, phoneme: 'p', opacity: 1 },
				{ year: 2025, phoneme: 'p', opacity: 1 }
			],
			evidence: [
				{ year: 1543, label: 'Portuguese: pan, tabako', detail: '' },
				{ year: 1900, label: 'English: pen, computer', detail: '' }
			],
			status: 'borrowed',
			pattern: 'introduced'
		}
	];

	// ─── Layout

	const flowById = new Map(flows.map((f) => [f.id, f]));

	const TIME_START = -3500;
	const TIME_END = 2025;
	const yearSpan = TIME_END - TIME_START;

	const bandHeight = 28;
	const laneGap = 6;
	const familyHeaderH = 38;
	const familyGap = 24;
	const plotPadX = 24;

	// Per family: compute lane range and Y offset
	const familyLayout = $derived.by(() => {
		const out: Record<string, { lanes: number[]; yTop: number; yBottom: number; yMid: number }> =
			{} as never;
		let y = 0;
		for (const fam of families) {
			const famFlows = flows.filter((f) => f.family === fam.id);
			const lanes = [...new Set(famFlows.map((f) => f.lane))].sort((a, b) => a - b);
			const minLane = Math.min(...lanes);
			const maxLane = Math.max(...lanes);
			const laneCount = maxLane - minLane + 1; // accounts for fractional via offsets
			const yTop = y + familyHeaderH;
			const yBottom = yTop + (maxLane - minLane) * (bandHeight + laneGap) + bandHeight;
			const yMid = (yTop + yBottom) / 2;
			out[fam.id] = { lanes, yTop, yBottom, yMid };
			y = yBottom + familyGap;
			void laneCount;
		}
		return out;
	});

	const totalHeight = $derived.by(() => {
		const lastFam = families.at(-1)!;
		return familyLayout[lastFam.id].yBottom + 36;
	});

	function flowY(flow: Flow): number {
		const layout = familyLayout[flow.family];
		const minLane = Math.min(...layout.lanes);
		return layout.yTop + (flow.lane - minLane) * (bandHeight + laneGap) + bandHeight / 2;
	}

	// ─── Pan/Zoom

	let zoom = $state(1);
	const basePlotWidth = 1400;
	const plotWidth = $derived(basePlotWidth * zoom);

	function x(year: number): number {
		return plotPadX + ((year - TIME_START) / yearSpan) * plotWidth;
	}

	function unX(px: number): number {
		return TIME_START + ((px - plotPadX) / plotWidth) * yearSpan;
	}

	const totalWidth = $derived(plotPadX * 2 + plotWidth);

	const ticks = [-3500, -3000, -2500, -2000, -1500, -1000, -500, 0, 500, 1000, 1500, 2000];

	function fmtYear(y: number): string {
		if (y === 0) return '0';
		return y < 0 ? `${-y} BCE` : `${y} CE`;
	}

	// ─── Ribbon path: draws a fork-in curve from parent (if any) then a horizontal band

	const FORK_YEARS = 300;

	function effectiveStops(flow: Flow): Stop[] {
		const stops = flow.stops;
		if (stops.length === 0) return stops;
		const first = stops[0];
		if (first.opacity === 0 || flow.fadeFrom !== undefined) return stops;
		const fadeYear = Math.min(first.year + FORK_YEARS, flow.end);
		return [
			{ year: first.year, phoneme: first.phoneme, opacity: 0 },
			{ year: fadeYear, phoneme: first.phoneme, opacity: 1 },
			...stops.filter((s, i) => i > 0 && s.year > fadeYear)
		];
	}

	function labelStartYear(flow: Flow): number {
		return flow.fadeFrom !== undefined ? flow.start : flow.start + FORK_YEARS;
	}

	function ribbonPath(flow: Flow): string {
		const yMid = flowY(flow);
		const yTop = yMid - bandHeight / 2;
		const yBot = yMid + bandHeight / 2;
		const startYear = flow.fadeFrom ?? flow.start;
		const xStart = x(startYear);
		const xEnd = x(flow.end);

		if (flow.parent) {
			const parent = flowById.get(flow.parent.id)!;
			const pY = flowY(parent);
			const pYTop = pY - bandHeight / 2;
			const pYBot = pY + bandHeight / 2;
			const xSplit = x(flow.parent.year);
			const xForkEnd = Math.min(x(flow.parent.year + FORK_YEARS), xEnd);
			const forkLen = xForkEnd - xSplit;
			const c1 = xSplit + forkLen * 0.5;
			const c2 = xForkEnd - forkLen * 0.5;

			return [
				`M ${xSplit} ${pYTop}`,
				`C ${c1} ${pYTop}, ${c2} ${yTop}, ${xForkEnd} ${yTop}`,
				`L ${xEnd} ${yTop}`,
				`L ${xEnd} ${yBot}`,
				`L ${xForkEnd} ${yBot}`,
				`C ${c2} ${yBot}, ${c1} ${pYBot}, ${xSplit} ${pYBot}`,
				`Z`
			].join(' ');
		}

		return [
			`M ${xStart} ${yTop}`,
			`L ${xEnd} ${yTop}`,
			`L ${xEnd} ${yBot}`,
			`L ${xStart} ${yBot}`,
			`Z`
		].join(' ');
	}

	// ─── Hover state

	let hovered: Flow | null = $state(null);
	let hoveredEv: { ev: Evidence; flow: Flow } | null = $state(null);

	function colorOf(phoneme: string): string {
		return PHONEME_COLOR[phoneme] ?? '#999';
	}

	// Distinct phonemes for legend
	const phonemesInUse = $derived.by(() => {
		const set = new Set<string>();
		for (const f of flows) for (const s of f.stops) set.add(s.phoneme);
		return [...set];
	});

	function setZoom(z: number) {
		zoom = Math.max(0.5, Math.min(8, z));
	}
</script>

<figure>
	<div class="controls">
		<div class="zoom">
			<button onclick={() => setZoom(zoom / 1.5)} aria-label="Zoom out">−</button>
			<span class="z">{Math.round(zoom * 100)}%</span>
			<button onclick={() => setZoom(zoom * 1.5)} aria-label="Zoom in">+</button>
			<button class="reset" onclick={() => setZoom(1)}>reset</button>
		</div>
		<div class="legend">
			{#each phonemesInUse as p (p)}
				<span class="sw" style:background={colorOf(p)}></span>
				<span class="sw-label">{p === '∅' ? '∅' : `/${p}/`}</span>
			{/each}
		</div>
	</div>

	<div class="pat-legend">
		<span class="pat-legend-title">Pattern shift</span>
		{#each Object.entries(PATTERN_INFO) as [key, info] (key)}
			<span class={`pat-item pat-${key}`} title={info.tip}>
				<span class="pat-glyph">{info.glyph}</span>
				<span class="pat-name">{info.label}</span>
			</span>
		{/each}
	</div>

	<div class="frame">
		<aside class="labels" style:height={`${totalHeight}px`}>
			{#each families as fam (fam.id)}
				{@const layout = familyLayout[fam.id]}
				<div
					class="fam-label"
					style:top={`${layout.yTop - familyHeaderH + 6}px`}
					style:height={`${layout.yBottom - layout.yTop + familyHeaderH}px`}
				>
					<div class="fam-name">{fam.name}</div>
					<div class="fam-sub">{fam.sub}</div>
				</div>
			{/each}
		</aside>

		<div class="scroll">
			<svg
				viewBox={`0 0 ${totalWidth} ${totalHeight}`}
				width={totalWidth}
				height={totalHeight}
				role="img"
				aria-label="Phonological flow diagram of /p/ loss across language families"
			>
				<defs>
					{#each flows as flow (flow.id)}
						{@const startYear = flow.fadeFrom ?? flow.start}
						{@const stops = effectiveStops(flow)}
						<linearGradient
							id={`grad-${flow.id}`}
							gradientUnits="userSpaceOnUse"
							x1={x(startYear)}
							y1={0}
							x2={x(flow.end)}
							y2={0}
						>
							{#each stops as stop, si (si)}
								{@const span = x(flow.end) - x(startYear)}
								{@const off = span > 0 ? ((x(stop.year) - x(startYear)) / span) * 100 : 0}
								<stop
									offset={`${Math.max(0, Math.min(100, off))}%`}
									stop-color={colorOf(stop.phoneme)}
									stop-opacity={stop.opacity ?? 1}
								/>
							{/each}
						</linearGradient>
					{/each}
				</defs>

				<!-- Family bands (subtle background) -->
				{#each families as fam, fi (fam.id)}
					{@const layout = familyLayout[fam.id]}
					<rect
						x={0}
						y={layout.yTop - familyHeaderH + 6}
						width={totalWidth}
						height={layout.yBottom - layout.yTop + familyHeaderH - 6}
						fill={fi % 2 === 0 ? '#fbf8f0' : '#f7f2e4'}
						opacity="0.5"
					/>
				{/each}

				<!-- Year grid -->
				<g class="axis">
					{#each ticks as t (t)}
						<line x1={x(t)} x2={x(t)} y1={0} y2={totalHeight - 24} />
						<text x={x(t)} y={totalHeight - 8} text-anchor="middle">{fmtYear(t)}</text>
					{/each}
					<line x1={x(0)} x2={x(0)} y1={0} y2={totalHeight - 24} class="zero" />
				</g>

				<!-- Ribbons -->
				{#each flows as flow (flow.id)}
					<g
						class="flow"
						class:hovered={hovered === flow}
						onmouseenter={() => (hovered = flow)}
						onmouseleave={() => (hovered = null)}
						role="button"
						tabindex="0"
					>
						<path d={ribbonPath(flow)} fill={`url(#grad-${flow.id})`} />
					</g>
				{/each}

				<!-- Flow labels (placed past the fade-in region) -->
				{#each flows as flow (flow.id)}
					{@const yMid = flowY(flow)}
					{@const lx = x(labelStartYear(flow)) + 6}
					{#if lx < x(flow.end) - 10}
						<text x={lx} y={yMid + 4} class="flow-label" pointer-events="none"
							>{flow.label}</text
						>
					{/if}
				{/each}

				<!-- End markers + pattern badges -->
				{#each flows as flow (flow.id)}
					{@const yMid = flowY(flow)}
					{@const xe = x(flow.end)}
					{#if flow.status === 'alive' || flow.status === 'borrowed'}
						<circle cx={xe + 6} cy={yMid} r="4" class="m-alive" />
					{:else if flow.status === 'dying'}
						<circle cx={xe + 6} cy={yMid} r="4" class="m-dying" />
					{:else}
						<g>
							<line x1={xe + 3} y1={yMid - 4} x2={xe + 9} y2={yMid + 4} class="m-dead" />
							<line x1={xe + 3} y1={yMid + 4} x2={xe + 9} y2={yMid - 4} class="m-dead" />
						</g>
					{/if}
					{#if flow.pattern}
						<g>
							<title>{PATTERN_INFO[flow.pattern].label}: {PATTERN_INFO[flow.pattern].tip}</title>
							<text
								x={xe + 18}
								y={yMid + 4}
								class={`pat-glyph-svg pat-${flow.pattern}`}
								pointer-events="all">{PATTERN_INFO[flow.pattern].glyph}</text
							>
						</g>
					{/if}
				{/each}

				<!-- Evidence dots -->
				{#each flows as flow (flow.id)}
					{#if flow.evidence}
						{@const yMid = flowY(flow)}
						{#each flow.evidence as ev (ev.year + ev.label)}
							<g
								class="ev"
								onmouseenter={() => (hoveredEv = { ev, flow })}
								onmouseleave={() => (hoveredEv = null)}
								role="button"
								tabindex="0"
							>
								<circle cx={x(ev.year)} cy={yMid} r="4" />
								<title>{fmtYear(ev.year)} — {ev.label}{ev.detail
										? ` (${ev.detail})`
										: ''}</title>
							</g>
						{/each}
					{/if}
				{/each}
			</svg>
		</div>
	</div>

	<!-- Detail panel -->
	<div class="detail" class:show={!!hovered || !!hoveredEv}>
		{#if hovered}
			<div class="dt-head">
				<strong>{hovered.label}</strong>
				{#if hovered.condition}<span class="dt-cond"> / {hovered.condition} /</span>{/if}
			</div>
			<div class="dt-meta">
				{families.find((f) => f.id === hovered!.family)!.name} ·
				<span class="mono">{fmtYear(hovered.start)} – {fmtYear(hovered.end)}</span> ·
				<span class={`st st-${hovered.status}`}>{hovered.status}</span>
				{#if hovered.pattern}
					· <span class="pat">
						<span class="pat-glyph">{PATTERN_INFO[hovered.pattern].glyph}</span>
						{PATTERN_INFO[hovered.pattern].label}
					</span>
				{/if}
			</div>
			{#if hovered.note}<div class="dt-note">{hovered.note}</div>{/if}
			{#if hovered.evidence && hovered.evidence.length}
				<ul class="dt-ev">
					{#each hovered.evidence as ev (ev.year + ev.label)}
						<li>
							<span class="mono">{fmtYear(ev.year)}</span> — {ev.label}{#if ev.detail}
								<span class="dt-detail"> ({ev.detail})</span>{/if}
						</li>
					{/each}
				</ul>
			{/if}
		{:else if hoveredEv}
			<div class="dt-head">
				<strong>{fmtYear(hoveredEv.ev.year)}</strong> — {hoveredEv.ev.label}
			</div>
			{#if hoveredEv.ev.detail}<div class="dt-note">{hoveredEv.ev.detail}</div>{/if}
			<div class="dt-meta">
				on lineage: <span class="mono">{hoveredEv.flow.label}</span>
			</div>
		{:else}
			<div class="dt-hint">hover a ribbon or evidence dot for details</div>
		{/if}
	</div>
</figure>

<style>
	figure {
		margin: 0;
		font-family: system-ui, sans-serif;
		--ink: #2a2620;
		--ink-soft: #6b6357;
		--rule: #d8d2c4;
		--bg: #fbf8f0;
	}

	.controls {
		display: flex;
		justify-content: space-between;
		align-items: center;
		gap: 16px;
		flex-wrap: wrap;
		margin-bottom: 8px;
		padding: 8px 12px;
		background: var(--bg);
		border: 1px solid var(--rule);
		border-radius: 6px;
	}
	.zoom {
		display: flex;
		align-items: center;
		gap: 6px;
		font-size: 13px;
	}
	.zoom button {
		width: 28px;
		height: 28px;
		border: 1px solid var(--rule);
		background: white;
		border-radius: 4px;
		cursor: pointer;
		font-size: 16px;
		color: var(--ink);
	}
	.zoom button.reset {
		width: auto;
		padding: 0 10px;
		font-size: 12px;
	}
	.zoom button:hover {
		background: #f0e8d0;
	}
	.zoom .z {
		min-width: 48px;
		text-align: center;
		font-family: ui-monospace, monospace;
		color: var(--ink-soft);
	}
	.legend {
		display: flex;
		align-items: center;
		gap: 4px 8px;
		flex-wrap: wrap;
		font-size: 12px;
	}
	.legend .sw {
		display: inline-block;
		width: 16px;
		height: 10px;
		border-radius: 2px;
		border: 1px solid rgba(0, 0, 0, 0.1);
	}
	.legend .sw-label {
		margin-right: 6px;
		font-family: ui-monospace, monospace;
		color: var(--ink);
	}

	.frame {
		display: grid;
		grid-template-columns: 180px 1fr;
		border: 1px solid var(--rule);
		border-radius: 6px;
		background: #fdfbf5;
		overflow: hidden;
	}

	.labels {
		position: relative;
		border-right: 1px solid var(--rule);
		background: var(--bg);
	}
	.fam-label {
		position: absolute;
		left: 0;
		right: 0;
		padding: 6px 12px;
		border-bottom: 1px dashed var(--rule);
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	.fam-name {
		font-size: 14px;
		font-weight: 600;
		color: var(--ink);
	}
	.fam-sub {
		font-size: 10.5px;
		color: var(--ink-soft);
		font-family: ui-monospace, monospace;
	}

	.scroll {
		overflow-x: auto;
		overflow-y: hidden;
	}
	.scroll svg {
		display: block;
	}

	.axis line {
		stroke: #e6dfd0;
		stroke-width: 0.75;
	}
	.axis line.zero {
		stroke: var(--rule);
		stroke-width: 1;
		stroke-dasharray: 3 2;
	}
	.axis text {
		font-size: 10px;
		fill: var(--ink-soft);
		font-family: ui-monospace, monospace;
	}

	.flow {
		cursor: pointer;
		transition: filter 0.15s ease;
	}
	.flow.hovered {
		filter: drop-shadow(0 1px 3px rgba(0, 0, 0, 0.2));
	}
	.flow-label {
		font-size: 11px;
		fill: #fff;
		font-weight: 600;
		font-family: ui-monospace, monospace;
		text-shadow:
			0 1px 1px rgba(0, 0, 0, 0.3),
			0 0 2px rgba(0, 0, 0, 0.4);
	}

	.m-alive {
		fill: #2d7a3e;
		stroke: white;
		stroke-width: 1.5;
	}
	.m-dying {
		fill: #b88a2e;
		stroke: white;
		stroke-width: 1.5;
	}
	.m-dead {
		stroke: #8a3d3d;
		stroke-width: 2;
		stroke-linecap: round;
	}

	.ev circle {
		fill: var(--ink);
		stroke: white;
		stroke-width: 1.5;
		cursor: help;
	}
	.ev:hover circle {
		fill: #d4a64a;
		r: 5;
	}

	.detail {
		margin-top: 10px;
		padding: 10px 14px;
		background: var(--bg);
		border: 1px solid var(--rule);
		border-radius: 6px;
		font-size: 13px;
		color: var(--ink);
		min-height: 60px;
	}
	.detail .dt-hint {
		color: var(--ink-soft);
		font-style: italic;
	}
	.dt-head {
		font-size: 14px;
	}
	.dt-cond {
		color: var(--ink-soft);
		font-family: ui-monospace, monospace;
		margin-left: 6px;
	}
	.dt-meta {
		margin-top: 4px;
		font-size: 12px;
		color: var(--ink-soft);
	}
	.dt-meta .mono {
		font-family: ui-monospace, monospace;
	}
	.dt-note {
		margin-top: 6px;
		font-size: 12.5px;
		color: var(--ink);
	}
	.dt-ev {
		margin: 6px 0 0;
		padding-left: 18px;
		font-size: 12px;
		color: var(--ink);
	}
	.dt-ev .mono {
		font-family: ui-monospace, monospace;
		color: var(--ink-soft);
	}
	.dt-detail {
		color: var(--ink-soft);
	}
	.st {
		text-transform: uppercase;
		font-size: 11px;
		font-weight: 600;
		letter-spacing: 0.05em;
	}
	.st-alive,
	.st-borrowed {
		color: #2d7a3e;
	}
	.st-dying {
		color: #b88a2e;
	}
	.st-dead {
		color: #8a3d3d;
	}

	/* Pattern badges */
	.pat-legend {
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: 8px 12px;
		margin: 10px 0 0;
		padding: 8px 12px;
		background: var(--bg);
		border: 1px solid var(--rule);
		border-radius: 6px;
		font-size: 12px;
	}
	.pat-legend-title {
		font-size: 11px;
		color: var(--ink-soft);
		text-transform: uppercase;
		letter-spacing: 0.1em;
		font-weight: 600;
		margin-right: 4px;
	}
	.pat-item {
		display: inline-flex;
		align-items: center;
		gap: 5px;
		cursor: help;
	}
	.pat-glyph {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		width: 20px;
		height: 20px;
		border-radius: 3px;
		font-family: ui-monospace, monospace;
		font-weight: 700;
		font-size: 12px;
	}
	.pat-name {
		color: var(--ink);
	}

	.pat-preserved .pat-glyph {
		background: #d4e8d4;
		color: #2d6a3d;
	}
	.pat-fricativization .pat-glyph {
		background: #d4dfee;
		color: #2c5a8a;
	}
	.pat-weakening .pat-glyph {
		background: #e8e0d0;
		color: #8a6a2d;
	}
	.pat-loss .pat-glyph {
		background: #e8d8d8;
		color: #8a3d3d;
	}
	.pat-restricted .pat-glyph {
		background: #ece4d8;
		color: #6b5a2a;
	}
	.pat-introduced .pat-glyph {
		background: #e0e0e8;
		color: #5a5a8a;
	}

	.pat {
		display: inline-flex;
		align-items: center;
		gap: 4px;
		text-transform: uppercase;
		font-size: 10.5px;
		letter-spacing: 0.06em;
		color: var(--ink-soft);
	}
	.pat .pat-glyph {
		width: 16px;
		height: 16px;
		font-size: 10.5px;
	}

	.pat-glyph-svg {
		font-family: ui-monospace, monospace;
		font-size: 12px;
		font-weight: 700;
		cursor: help;
	}
	.pat-glyph-svg.pat-preserved {
		fill: #2d6a3d;
	}
	.pat-glyph-svg.pat-fricativization {
		fill: #2c5a8a;
	}
	.pat-glyph-svg.pat-weakening {
		fill: #8a6a2d;
	}
	.pat-glyph-svg.pat-loss {
		fill: #8a3d3d;
	}
	.pat-glyph-svg.pat-restricted {
		fill: #6b5a2a;
	}
	.pat-glyph-svg.pat-introduced {
		fill: #5a5a8a;
	}
</style>
