<script>
	import { onMount } from 'svelte';
	let userInput = $state();
	let secretNumber = $state(Math.floor(Math.random() * 10) + 1);
	let output = $state('Bitte gib eine Zahl ein');
	let disabled = $state(true);
	console.log(secretNumber)

	function checkGuess(){
		const guess = Number(userInput);
		disabled = true;
		console.log('guess');
		if(guess === secretNumber){
			output = "Getroffen!";
			disabled = false;
		}
		else if(guess < secretNumber){
			output = "Die gesuchte Zahl ist grösser";
		}
		else if(guess > secretNumber){
			output = "Die gesuchte Zahl ist kleiner";
		}
	}

	function resetGame(){
		secretNumber = Math.floor(Math.random() * 10) + 1;
		output = "Bitte gib eine Zahl ein";
		userInput = null;
	}
</script>


<h1>Guessing Game</h1>
<p>Errate die Geheimzahl zwischen 1 und 10</p>
<input type="number" bind:value={userInput} min="1" max="10" />
<button type="submit" onclick={checkGuess}>Rate</button>
<button type="reset" disabled={disabled} onclick={resetGame}>Spiel neustarten</button>
<p>{output}</p>
