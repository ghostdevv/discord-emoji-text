<script>
	import { fade } from 'svelte/transition';

	const opt = {
		symbols: true,
		letters: true,
		nitroEmoji: true,
		numbers: true,
		unknown: true,

		showInfo: false
	};

	const data = {
		input: 'Hello World',
		output: '',

		outputElement: undefined
	};

	$: data.output = convertToEmojiString(data.input, opt);

	let globalQuoteCounter = 0;

	function convertToEmojiString(string, opt) {
		globalQuoteCounter = 0;
		return [...string].map(x => x == '\n' ? '\n' : x.toEmoji(opt) + (opt.nitroEmoji ? '' : ' ')).join('').trim();
	};

	function copyOutput() {
		data.outputElement.focus();
		data.outputElement.select();
		try {
			const x = document.execCommand('copy');
			if (!x) throw x;
		}
		catch {
			alert('There was a error in copying');
		};
	};

	const convertionData = {
		letters: {
			A: [ ':A1:', ':regional_indicator_a:' ],
			B: [ ':B1:', ':regional_indicator_b:' ],
			C: [ ':C1:', ':regional_indicator_c:' ],
			D: [ ':D1:', ':regional_indicator_d:' ],
			E: [ ':E1:', ':regional_indicator_e:' ],
			F: [ ':F1:', ':regional_indicator_f:' ],
			G: [ ':G1:', ':regional_indicator_g:' ],
			H: [ ':H1:', ':regional_indicator_h:' ],
			I: [ ':I1:', ':regional_indicator_i:' ],
			J: [ ':J1:', ':regional_indicator_j:' ],
			K: [ ':K1:', ':regional_indicator_k:' ],
			L: [ ':L1:', ':regional_indicator_l:' ],
			M: [ ':M1:', ':regional_indicator_m:' ],
			N: [ ':N1:', ':regional_indicator_n:' ],
			O: [ ':O1:', ':regional_indicator_o:' ],
			P: [ ':P1:', ':regional_indicator_p:' ],
			Q: [ ':Q1:', ':regional_indicator_q:' ],
			R: [ ':R1:', ':regional_indicator_r:' ],
			S: [ ':S1:', ':regional_indicator_s:' ],
			T: [ ':T1:', ':regional_indicator_t:' ],
			U: [ ':U1:', ':regional_indicator_u:' ],
			V: [ ':V1:', ':regional_indicator_v:' ],
			W: [ ':W1:', ':regional_indicator_w:' ],
			X: [ ':X1:', ':regional_indicator_x:' ],
			Y: [ ':Y1:', ':regional_indicator_y:' ],
			Z: [ ':Z1:', ':regional_indicator_z:' ]
		},
		numbers: {
			'0': [ ':01:', ':zero:' ],
			'1': [ ':11:', ':one:' ],
			'2': [ ':21:', ':two:' ],
			'3': [ ':31:', ':three:' ],
			'4': [ ':41:', ':four:' ],
			'5': [ ':51:', ':five:' ],
			'6': [ ':61:', ':six:' ],
			'7': [ ':71:', ':seven:' ],
			'8': [ ':81:', ':eight:' ],
			'9': [ ':91:', ':nine:' ]
		},
		symbols: {
			' ': [ ':_1:', ':blue_square:' ],
			'.': [ ':b1:', ':small_blue_diamond:' ],
			',': [ ':c1:', ':small_red_triangle_down:' ],
			'!': [ ':d1:', ':exclamation:' ],
			'?': [ ':e1:', ':question:' ],
			'"': [ ':f1:', ':pause_button: ' ],
			"'": [ ':f1:', ':pause_button: ' ]
		},
		else: [':a1:', ':record_button:']
	};

	const convertionKeys = {
		letters: () => Object.keys(convertionData.letters),
		numbers: () => Object.keys(convertionData.numbers),
		symbols: () => Object.keys(convertionData.symbols)
	};

	String.prototype.toEmoji = function(opt) {
		if (this == '\n') return this;

		const char = this.toUpperCase();
		let choice;

		if (convertionKeys.letters().includes(char)) {
			if (opt.letters) choice = convertionData.letters[char];
		} else if (convertionKeys.numbers().includes(char)) {
			if (opt.numbers) choice = convertionData.numbers[char];
		} else if (convertionKeys.symbols().includes(char)) {
			if (opt.symbols) {
				choice = convertionData.symbols[char];
				if (['"', "'"].includes(char)) {
					choice[0] = globalQuoteCounter % 2 == 0 ? ':f1:' : ':g1:';
					globalQuoteCounter++;
				};
			};
		} else {
			if (opt.unknown) choice = convertionData.else;
		};

		return choice ? opt.nitroEmoji ? choice[0] : choice[1] : this;
	};
</script>

<main>
	<span>Text to Emoji</span>
	<div class="options">
		<div>
			<input type="checkbox" id="symbols" bind:checked={opt.symbols} />
			<label for="symbols">Symbols</label>
		</div>
		<div>
			<input type="checkbox" id="numbers" bind:checked={opt.numbers} />
			<label for="numbers">Numbers</label>
		</div>
		<div>
			<input type="checkbox" id="letters" bind:checked={opt.letters} />
			<label for="letters">Letters</label>
		</div>
		<div>
			<input type="checkbox" id="unknown" bind:checked={opt.unknown} />
			<label for="unknown">Convert unsupported chars to unknown</label>
		</div>
		<div>
			<input type="checkbox" id="nitro" bind:checked={opt.nitroEmoji} />
			<label for="nitro">Use nitro emojis</label>
		</div>
	</div>
	<div class="buttons">
		<button on:click={() => data.input = ''}>Clear</button>
		<button on:click={copyOutput}>Copy Output</button>
	</div>
	<div class="input">
		<textarea bind:value={data.input}></textarea>
		<textarea bind:this={data.outputElement} bind:value={data.output} ></textarea>
	</div>
</main>

<div class="info">
	{#if opt.showInfo}
		<div transition:fade>
			<span>Made by GHOST, using HTML, SCSS, JavaScript, and Svelte</span>
		</div>
	{/if}
	<div class="button" on:click={() => opt.showInfo = !opt.showInfo}>
		<svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
			<path d="M11 10.9794C11 10.4271 11.4477 9.97937 12 9.97937C12.5523 9.97937 13 10.4271 13 10.9794V16.9794C13 17.5317 12.5523 17.9794 12 17.9794C11.4477 17.9794 11 17.5317 11 16.9794V10.9794Z" fill="currentColor" />
			<path d="M12 6.05115C11.4477 6.05115 11 6.49886 11 7.05115C11 7.60343 11.4477 8.05115 12 8.05115C12.5523 8.05115 13 7.60343 13 7.05115C13 6.49886 12.5523 6.05115 12 6.05115Z" fill="currentColor" />
			<path fill-rule="evenodd" clip-rule="evenodd" d="M12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2ZM4 12C4 16.4183 7.58172 20 12 20C16.4183 20 20 16.4183 20 12C20 7.58172 16.4183 4 12 4C7.58172 4 4 7.58172 4 12Z" fill="currentColor" />
	  </svg>
	</div>
</div>

<style lang="scss">
	.info {
		position: absolute;
		top: 0;
		right: 0;
		padding: 4px;

		display: flex;
		align-items: center;

		> * {
			padding: 0px 4px;
		}

		.button {
			cursor: pointer;
			opacity: 0.6;
		}
		
		.button:hover {
			opacity: 1;
		}
	}

	main {
		width: 100%;
		height: 100%;

		display: flex;
		flex-flow: column nowrap;

		span {
			font-size: 2rem;
			font-weight: 300;
			padding: 2px;
		}

		> * {
			width: 100%;
			padding: 2px 4px;
		}

		.options {
			display: flex;

			> * {
				display: flex;
				align-items: center;
				padding: 0px 2px;
				
				> * {
					padding: 0px 4px;
				}
			}
		}

		.input {
			flex-grow: 1;

			display: flex;
			flex-flow: row nowrap;

			textarea {
				height: 100%;
				flex-grow: 1;
				resize: none;
				color: #000;
				margin: 0px 2px;
			}

			textarea:first-child {
				margin-left: 0px;
			}

			textarea:last-child {
				margin-right: 0px;
			}

			textarea:focus {
				outline-color: #0075ff;
			}
		}
	}
</style>