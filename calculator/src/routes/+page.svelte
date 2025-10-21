<script>
	let output = $state("0"); // Variable für die Anzeige der Berechnung bzw. des Resultats der Berechnung
	let operand = $state(null);
	let number1 = $state(null);
	let number2 = $state(null);
	function setDigit(digit) {
		if(operand == null){
			if(digit !== 0) {
				// Wenn die Anzeige nur aus einer "0" besteht, wird diese durch die neue Zahl ersetzt
				// Ansonsten wird die Zahl an die Anzeige angehängt
				if(output.length === 1 && output.startsWith("0") ) {
					output = digit;
				}else {
					output = output + digit.toString();
				}
				number1 = output;
			}
		}
		else{
			output = output + digit.toString();
			if (number2 == null){
				number2 = digit.toString();
			}
			else{
				number2 = number2 + digit.toString();
			}
		}
	}

	function setOperation(op) {
		console.log(op);
		if(operand === null){
			operand = op.toString();
			output = output + op;
			number2 = null;
		} else {
			// If there's already an operand and we have both numbers, calculate first
			if(number1 !== null && number2 !== null) {
				calculate();
			}
			operand = op.toString();
			output = output + op;
			number2 = null;
		}
	}



	function calculate() {
		if(operand.startsWith("+")){
			output = parseFloat(number1) + parseFloat(number2);
		}
		else if(operand.startsWith("-")){
			output = parseFloat(number1) - parseFloat(number2);
		}
		else if(operand.startsWith("*")){
			output = parseFloat(number1) * parseFloat(number2);
		}
		else if(operand.startsWith("/")){
			output = parseFloat(number1) / parseFloat(number2);
		}

		operand = null;
		number1 = output;
	}

	function allClear() {
		output = "0";
		number1 = null;
		number2 = null;
		operand = null;
	}
</script>

<div class="calculator">
	<div id="display">{output}</div>

	<div class="keypad">
		<button class="key" onclick={() => { setDigit(7); }}>7</button>
		<button class="key" onclick={() => { setDigit(8); }}>8</button>
		<button class="key" onclick={() => { setDigit(9); }}>9</button>
		<button class="key operation" onclick={() => { setOperation("/"); }}>/</button>

		<button class="key" onclick={() => { setDigit(4); }}>4</button>
		<button class="key" onclick={() => { setDigit(5); }}>5</button>
		<button class="key" onclick={() => { setDigit(6); }}>6</button>
		<button class="key operation" onclick={() => { setOperation("*"); }}>*</button>

		<button class="key" onclick={() => { setDigit(1); }}>1</button>
		<button class="key" onclick={() => { setDigit(2); }}>2</button>
		<button class="key" onclick={() => { setDigit(3); }}>3</button>
		<button class="key operation" onclick={() => { setOperation("-"); }}>-</button>

		<button class="key" onclick={() => { setDigit(0); }}>0</button>
		<button class="key operation" onclick={allClear}>AC</button>
		<button class="key operation" onclick={calculate}>=</button>
		<button class="key operation" onclick={() => { setOperation("+"); }}>+</button>
	</div>
</div>

<style>
    /* Container-Element für den gesamten Rechner */
    .calculator {
        background-color: black;
        padding: 1em;
        /* Breite ohne padding: 4xTastenbreite + 3xGap */
        width: 13.5em;
    }

    /* Zahlenanzeige */
    #display {
        color: white;
        font-weight: bold;
        background-color: black;

        border: 1px solid white;
        padding: 0.5em;
        text-align: end;
        margin-bottom: 0.5em;
        font-family: "Courier New", Courier, monospace;
        font-size: 1.3em;
    }

    /* Tasten als Grid. Dokumentation zu Grid: https://www.w3schools.com/CSSref/pr_grid.asp */
    .keypad {
        display: grid;
        grid-template-columns: repeat(4, 3em);
        grid-template-rows: repeat(4, 3em);
        row-gap: 0.5em;
        column-gap: 0.5em;
    }

    /* Einzelne Tasten */
    .key {
        background-color: gray;
        color: white;
        font-family: "Courier New", Courier, monospace;
        font-size: 1.2em;
        font-weight: bold;

        justify-content: center;
        align-items: center;
        border-radius: 100%;
        border: none;
    }

    /* Mit der :active Pseudoklasse kann das Aussehen des Buttons verändert werden, solange er gedrückt wird.
	 Siehe https://www.w3schools.com/cssref/sel_active.asp */
    .key:active {
        background-color: lightgrey;
    }

    /* Spezialfarbe für Operationstasten */
    .operation {
        background-color: orangered;
    }

    /* Aussehen eines gerade geklickten Operators (+,-,*,/) */
    .operation:active {
        background-color: white;
        color: orangered;
    }
</style>

