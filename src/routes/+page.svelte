<script lang="ts">
	let number = 0
	let finalNum = 0
	let multiplicador = 0
	function calcular(){
		finalNum = number * multiplicador; 
		console.log(finalNum)
	}
</script>

<svelte:head>
	<title>Costos mp</title>
	<meta name="description" content="Calculadora costos mp" />
</svelte:head>

<section>
	<h1>
		Calcular monto de qr mp
	</h1>
	<select name="selec" id="selec" bind:value={multiplicador}>
		<option value="1.008">dinero en cuenta</option>
		<option value="1.0135">débito</option>
		<option value="1.07999">tarjeta crédito</option>
		<option value="1.0135">mercado crédito</option>
	</select>
	<form>
		<input type="number" bind:value={number}>
		<button on:click={calcular}>
			Calcular
		</button>
	</form>
	<h2>Tiene que pagar: {finalNum}</h2>
</section>
