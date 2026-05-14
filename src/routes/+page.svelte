<script lang="ts">
	// ── Calculadora ──────────────────────────────────────────────
	let number: number | null = null
	let multiplicador = '1.008'
	let finalNum: number | null = null

	const metodos = [
		{ label: 'Dinero en cuenta',   value: '1.008',   comision: '0.8%' },
		{ label: 'Débito',             value: '1.0135',  comision: '1.35%' },
		{ label: 'Tarjeta de crédito', value: '1.07999', comision: '8%' },
		{ label: 'Mercado Crédito',    value: '1.0135',  comision: '1.35%' },
	]

	$: metodoSeleccionado = metodos.find(m => m.value === multiplicador)!

	function calcular() {
		if (number !== null && number > 0) {
			finalNum = Math.round(number * parseFloat(multiplicador) * 100) / 100
		}
	}

	function formatPesos(n: number) {
		return n.toLocaleString('es-AR', { style: 'currency', currency: 'ARS' })
	}

	function handleKeydown(e: KeyboardEvent) {
		if (e.key === 'Enter') calcular()
	}

	// ── Examen ───────────────────────────────────────────────────
	type Question = {
		id: number
		category: 'Comparative' | 'Superlative' | 'Adverb'
		text: string
		options: string[]
		correct: number
	}

	const questions: Question[] = [
		// Comparatives
		{
			id: 1, category: 'Comparative',
			text: 'She is _____ than her sister.',
			options: ['more tall', 'taller', 'tallest', 'the tallest'],
			correct: 1,
		},
		{
			id: 2, category: 'Comparative',
			text: 'This exercise is _____ than the previous one.',
			options: ['difficulter', 'more difficult', 'most difficult', 'the most difficult'],
			correct: 1,
		},
		{
			id: 3, category: 'Comparative',
			text: 'Today is _____ than yesterday.',
			options: ['more hot', 'hotest', 'hotter', 'the hottest'],
			correct: 2,
		},
		{
			id: 4, category: 'Comparative',
			text: 'My phone is _____ than yours.',
			options: ['expensiver', 'most expensive', 'the most expensive', 'more expensive'],
			correct: 3,
		},
		{
			id: 5, category: 'Comparative',
			text: 'He drives _____ than his father.',
			options: ['more careful', 'carefuller', 'more carefully', 'carefullier'],
			correct: 2,
		},
		{
			id: 6, category: 'Comparative',
			text: 'This road is _____ than the highway.',
			options: ['dangerouser', 'the most dangerous', 'most dangerous', 'more dangerous'],
			correct: 3,
		},
		{
			id: 7, category: 'Comparative',
			text: 'She works _____ than anyone in the office.',
			options: ['more hard', 'hardest', 'the hardest', 'harder'],
			correct: 3,
		},
		// Superlatives
		{
			id: 8, category: 'Superlative',
			text: 'This is _____ restaurant in the city.',
			options: ['the better', 'the most good', 'better', 'the best'],
			correct: 3,
		},
		{
			id: 9, category: 'Superlative',
			text: 'Everest is _____ mountain in the world.',
			options: ['higher', 'the higher', 'the most high', 'the highest'],
			correct: 3,
		},
		{
			id: 10, category: 'Superlative',
			text: 'She is _____ student in the class.',
			options: ['more intelligent', 'intelligenter', 'most intelligent', 'the most intelligent'],
			correct: 3,
		},
		{
			id: 11, category: 'Superlative',
			text: 'This is _____ day of my life!',
			options: ['the most bad', 'the worse', 'the worst', 'the baddest'],
			correct: 2,
		},
		{
			id: 12, category: 'Superlative',
			text: 'He is _____ singer I\'ve ever heard.',
			options: ['more talented', 'talentedest', 'the most talented', 'most talented'],
			correct: 2,
		},
		{
			id: 13, category: 'Superlative',
			text: 'Of all my friends, Ana lives _____ away.',
			options: ['more far', 'farther', 'most far', 'the farthest'],
			correct: 3,
		},
		{
			id: 14, category: 'Superlative',
			text: 'That was _____ film I\'ve ever watched.',
			options: ['more boring', 'boringly', 'the boringest', 'the most boring'],
			correct: 3,
		},
		// Adverbs
		{
			id: 15, category: 'Adverb',
			text: 'She speaks English _____.',
			options: ['good', 'goodly', 'well', 'gooder'],
			correct: 2,
		},
		{
			id: 16, category: 'Adverb',
			text: 'The children played _____ in the garden.',
			options: ['happy', 'happiness', 'happifully', 'happily'],
			correct: 3,
		},
		{
			id: 17, category: 'Adverb',
			text: 'Please drive _____ — the roads are icy.',
			options: ['slow', 'slowly', 'more slow', 'slowy'],
			correct: 1,
		},
		{
			id: 18, category: 'Adverb',
			text: 'He explained everything so _____ that everyone understood.',
			options: ['clear', 'clearing', 'cleary', 'clearly'],
			correct: 3,
		},
		{
			id: 19, category: 'Adverb',
			text: 'She finished the test _____ than expected.',
			options: ['more quick', 'quicklier', 'more quickly', 'quickest'],
			correct: 2,
		},
		{
			id: 20, category: 'Adverb',
			text: 'Of all the runners, Tom finished _____.',
			options: ['more fast', 'faster', 'most fast', 'the fastest'],
			correct: 3,
		},
	]

	const categoryColor: Record<Question['category'], string> = {
		Comparative: 'bg-violet-100 text-violet-700',
		Superlative:  'bg-amber-100  text-amber-700',
		Adverb:       'bg-emerald-100 text-emerald-700',
	}

	let answers: (number | null)[] = Array(questions.length).fill(null)
	let submitted = false
	let score = 0

	function submitExam() {
		score = answers.reduce<number>((acc, ans, i) => acc + (ans === questions[i].correct ? 1 : 0), 0)
		submitted = true
		setTimeout(() => {
			document.getElementById('resultado')?.scrollIntoView({ behavior: 'smooth' })
		}, 50)
	}

	function resetExam() {
		answers = Array(questions.length).fill(null)
		submitted = false
		score = 0
	}

	$: allAnswered = answers.every(a => a !== null)

	const optionLabels = ['A', 'B', 'C', 'D']
</script>

<svelte:head>
	<title>Costos MP</title>
	<meta name="description" content="Calculadora de costos Mercado Pago" />
</svelte:head>

<div class="min-h-screen bg-slate-100 p-4 pb-16">
	<div class="w-full max-w-xl mx-auto space-y-10">

		<!-- ── Calculadora ────────────────────────────────────── -->
		<div>
			<div class="text-center mb-6">
				<div class="inline-flex items-center justify-center w-14 h-14 rounded-2xl bg-sky-500 mb-3 shadow-lg">
					<svg class="w-7 h-7 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
						<path stroke-linecap="round" stroke-linejoin="round" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 11h.01M12 11h.01M15 11h.01M4 19h16a2 2 0 002-2V7a2 2 0 00-2-2H4a2 2 0 00-2 2v10a2 2 0 002 2z" />
					</svg>
				</div>
				<h1 class="text-2xl font-bold text-slate-800">Calculadora MP</h1>
				<p class="text-slate-500 text-sm mt-1">Calculá el monto con comisiones de Mercado Pago</p>
			</div>

			<div class="bg-white rounded-2xl shadow-xl overflow-hidden">
				<div class="bg-slate-800 px-6 py-5">
					<p class="text-slate-400 text-xs uppercase tracking-widest mb-1">Monto a cobrar</p>
					<p class="text-white text-4xl font-light tracking-tight">
						{#if finalNum !== null}
							{formatPesos(finalNum)}
						{:else}
							<span class="text-slate-500">$ —</span>
						{/if}
					</p>
					{#if finalNum !== null && number}
						<p class="text-slate-400 text-xs mt-2">
							{formatPesos(number)} + {metodoSeleccionado.comision} ({metodoSeleccionado.label})
						</p>
					{/if}
				</div>

				<div class="p-6 space-y-4">
					<div>
						<label for="metodo" class="block text-xs font-semibold text-slate-500 uppercase tracking-wide mb-1.5">
							Método de pago
						</label>
						<div class="relative">
							<select
								id="metodo"
								bind:value={multiplicador}
								on:change={() => { finalNum = null }}
								class="w-full appearance-none bg-slate-50 border border-slate-200 text-slate-800 rounded-xl px-4 py-3 pr-10 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-sky-400 focus:border-transparent cursor-pointer"
							>
								{#each metodos as m}
									<option value={m.value}>{m.label} — {m.comision}</option>
								{/each}
							</select>
							<div class="pointer-events-none absolute inset-y-0 right-3 flex items-center">
								<svg class="w-4 h-4 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
									<path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
								</svg>
							</div>
						</div>
					</div>

					<div>
						<label for="monto" class="block text-xs font-semibold text-slate-500 uppercase tracking-wide mb-1.5">
							Monto base ($)
						</label>
						<div class="relative">
							<span class="absolute left-4 top-1/2 -translate-y-1/2 text-slate-400 font-medium">$</span>
							<input
								id="monto"
								type="number"
								min="0"
								step="0.01"
								placeholder="0.00"
								bind:value={number}
								on:keydown={handleKeydown}
								on:input={() => { finalNum = null }}
								class="w-full bg-slate-50 border border-slate-200 text-slate-800 rounded-xl pl-8 pr-4 py-3 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-sky-400 focus:border-transparent"
							/>
						</div>
					</div>

					<button
						on:click={calcular}
						class="w-full bg-sky-500 hover:bg-sky-600 active:bg-sky-700 text-white font-semibold rounded-xl py-3.5 text-sm transition-colors duration-150 shadow-md shadow-sky-200"
					>
						Calcular
					</button>
				</div>
			</div>
		</div>

		<!-- ── Examen ─────────────────────────────────────────── -->
		<div>
			<div class="text-center mb-6">
				<div class="inline-flex items-center justify-center w-14 h-14 rounded-2xl bg-indigo-600 mb-3 shadow-lg">
					<svg class="w-7 h-7 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
						<path stroke-linecap="round" stroke-linejoin="round" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
					</svg>
				</div>
				<h2 class="text-2xl font-bold text-slate-800">English Grammar Quiz</h2>
				<p class="text-slate-500 text-sm mt-1">Adverbs · Comparatives · Superlatives — 20 points</p>

				<div class="flex justify-center gap-3 mt-4 flex-wrap">
					<span class="text-xs font-medium px-3 py-1 rounded-full bg-violet-100 text-violet-700">Comparative ×7</span>
					<span class="text-xs font-medium px-3 py-1 rounded-full bg-amber-100  text-amber-700">Superlative ×7</span>
					<span class="text-xs font-medium px-3 py-1 rounded-full bg-emerald-100 text-emerald-700">Adverb ×6</span>
				</div>
			</div>

			<div class="space-y-4">
				{#each questions as q, i}
					{@const chosen   = answers[i]}
					{@const isRight  = submitted && chosen === q.correct}
					{@const isWrong  = submitted && chosen !== null && chosen !== q.correct}

					<div class="bg-white rounded-2xl shadow-sm border {submitted ? (isRight ? 'border-emerald-300' : isWrong ? 'border-red-300' : 'border-slate-200') : 'border-slate-200'} overflow-hidden">
						<!-- Question header -->
						<div class="px-5 pt-4 pb-3 flex items-start gap-3">
							<span class="flex-shrink-0 w-7 h-7 rounded-full bg-indigo-600 text-white text-xs font-bold flex items-center justify-center mt-0.5">{q.id}</span>
							<div class="flex-1 min-w-0">
								<span class="inline-block text-xs font-semibold px-2 py-0.5 rounded-full mb-1.5 {categoryColor[q.category]}">{q.category}</span>
								<p class="text-slate-800 font-medium text-sm leading-snug">{q.text}</p>
							</div>
							{#if submitted}
								<span class="flex-shrink-0 text-lg">{isRight ? '✓' : isWrong ? '✗' : '—'}</span>
							{/if}
						</div>

						<!-- Options -->
						<div class="px-5 pb-4 space-y-2">
							{#each q.options as opt, j}
								{@const isChosen  = chosen === j}
								{@const isCorrect = j === q.correct}

								<label class="flex items-center gap-3 rounded-xl px-3 py-2.5 cursor-pointer transition-colors
									{submitted
										? isCorrect
											? 'bg-emerald-50 border border-emerald-300'
											: isChosen && !isCorrect
												? 'bg-red-50 border border-red-300'
												: 'bg-slate-50 border border-slate-100'
										: isChosen
											? 'bg-indigo-50 border border-indigo-300'
											: 'bg-slate-50 border border-slate-100 hover:border-indigo-200 hover:bg-indigo-50/50'}
								">
									<input
										type="radio"
										name="q{q.id}"
										value={j}
										bind:group={answers[i]}
										disabled={submitted}
										class="accent-indigo-600 w-4 h-4 flex-shrink-0"
									/>
									<span class="text-xs font-bold text-slate-400 w-4">{optionLabels[j]}</span>
									<span class="text-sm text-slate-700">{opt}</span>
								</label>
							{/each}
						</div>
					</div>
				{/each}
			</div>

			<!-- Submit / resultado -->
			{#if !submitted}
				<div class="mt-6">
					{#if !allAnswered}
						<p class="text-center text-slate-400 text-xs mb-3">
							Respondé todas las preguntas para enviar ({answers.filter(a => a !== null).length}/20)
						</p>
					{/if}
					<button
						on:click={submitExam}
						disabled={!allAnswered}
						class="w-full py-3.5 rounded-xl font-semibold text-sm transition-colors duration-150
							{allAnswered
								? 'bg-indigo-600 hover:bg-indigo-700 active:bg-indigo-800 text-white shadow-md shadow-indigo-200'
								: 'bg-slate-200 text-slate-400 cursor-not-allowed'}"
					>
						Enviar examen
					</button>
				</div>
			{:else}
				<div id="resultado" class="mt-6 bg-white rounded-2xl shadow-xl overflow-hidden">
					<div class="px-6 py-6 text-center
						{score >= 18 ? 'bg-emerald-600' : score >= 14 ? 'bg-sky-600' : score >= 10 ? 'bg-amber-500' : 'bg-red-500'}
					">
						<p class="text-white/80 text-sm font-medium uppercase tracking-widest mb-1">Tu puntaje</p>
						<p class="text-white text-6xl font-bold">{score}<span class="text-3xl font-light">/20</span></p>
						<p class="text-white/90 text-base mt-2 font-medium">
							{#if score === 20}
								¡Perfecto! 🎉
							{:else if score >= 18}
								¡Excelente!
							{:else if score >= 14}
								¡Muy bien!
							{:else if score >= 10}
								Bien, podés mejorar
							{:else}
								Seguí practicando
							{/if}
						</p>
					</div>
					<div class="px-6 py-4 flex justify-between text-sm text-slate-600">
						<span>✓ Correctas: <strong class="text-emerald-600">{score}</strong></span>
						<span>✗ Incorrectas: <strong class="text-red-500">{20 - score}</strong></span>
					</div>
					<div class="px-6 pb-5">
						<button
							on:click={resetExam}
							class="w-full py-3 rounded-xl border-2 border-indigo-200 text-indigo-600 font-semibold text-sm hover:bg-indigo-50 transition-colors"
						>
							Intentar de nuevo
						</button>
					</div>
				</div>
			{/if}
		</div>

	</div>
</div>
