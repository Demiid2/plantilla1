<script lang="ts">
	let number: number | null = null
	let multiplicador = '1.008'
	let finalNum: number | null = null

	const metodos = [
		{ label: 'Dinero en cuenta', value: '1.008', comision: '0.8%' },
		{ label: 'Débito',           value: '1.0135', comision: '1.35%' },
		{ label: 'Tarjeta de crédito', value: '1.07999', comision: '8%' },
		{ label: 'Mercado Crédito',  value: '1.0135', comision: '1.35%' },
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
</script>

<svelte:head>
	<title>Costos MP</title>
	<meta name="description" content="Calculadora de costos Mercado Pago" />
</svelte:head>

<div class="min-h-screen bg-slate-100 flex items-center justify-center p-4">
	<div class="w-full max-w-sm">

		<!-- Header -->
		<div class="text-center mb-6">
			<div class="inline-flex items-center justify-center w-14 h-14 rounded-2xl bg-sky-500 mb-3 shadow-lg">
				<svg class="w-7 h-7 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
					<path stroke-linecap="round" stroke-linejoin="round" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 11h.01M12 11h.01M15 11h.01M4 19h16a2 2 0 002-2V7a2 2 0 00-2-2H4a2 2 0 00-2 2v10a2 2 0 002 2z" />
				</svg>
			</div>
			<h1 class="text-2xl font-bold text-slate-800">Calculadora MP</h1>
			<p class="text-slate-500 text-sm mt-1">Calculá el monto con comisiones de Mercado Pago</p>
		</div>

		<!-- Card -->
		<div class="bg-white rounded-2xl shadow-xl overflow-hidden">

			<!-- Display -->
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

			<!-- Inputs -->
			<div class="p-6 space-y-4">

				<!-- Método de pago -->
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

				<!-- Monto -->
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

				<!-- Botón -->
				<button
					on:click={calcular}
					class="w-full bg-sky-500 hover:bg-sky-600 active:bg-sky-700 text-white font-semibold rounded-xl py-3.5 text-sm transition-colors duration-150 shadow-md shadow-sky-200"
				>
					Calcular
				</button>

			</div>
		</div>

	</div>
</div>
