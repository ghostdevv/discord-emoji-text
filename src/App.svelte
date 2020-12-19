<script>
	import { fade } from 'svelte/transition';

	const opt = {
		spaces: true,
		nitroEmoji: true,
		numbers: true,
		unkown: true,

		showInfo: false
	};

	const data = {
		input: 'Hello World',
		output: ''
	};

	$: data.output = convertToEmojiString(data.input, opt);

	function convertToEmojiString(string, opt) {
		return [...string].map(x => x.toEmoji(opt)).join(opt.nitroEmoji ? '' : ' ');
	};

	const numbersAsWords = {
		1: 'one',
		2: 'two',
		3: 'three',
		4: 'four',
		5: 'five',
		6: 'six',
		7: 'seven',
		8: 'eight',
		9: 'nine',
		0: 'zero'
	};

	String.prototype.toEmoji = function(opt) {
		if (opt.nitroEmoji) {
			if (this == ' ') return opt.spaces ? ':_1:' : ' ';
			if (!isNaN(Number(this)) && !opt.numbers) return this;
			if (![...'ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890 '].includes(this.toUpperCase())) return opt.unkown ? ':a1:' : this;
			return `:${this.toUpperCase()}1:`;
		} else {
			if (this == ' ') return opt.spaces ? ':blue_square:' : ' ';
			if (!isNaN(Number(this)) && opt.numbers) return `:${numbersAsWords[Number(this)]}:`;
			if (![...'ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890 '].includes(this.toUpperCase())) return opt.unkown ? ':record_button:' : this;
			return `:regional_indicator_${this.toLowerCase()}:`;
		};
	};
</script>

<main>
	<span>Text to Emoji</span>
	<div class="options">
		<div>
			<input type="checkbox" id="spaces" bind:checked={opt.spaces} />
			<label for="spaces">Convert spaces to emoji</label>
		</div>
		<div>
			<input type="checkbox" id="numbers" bind:checked={opt.numbers} />
			<label for="numbers">Convert numbers to emoji</label>
		</div>
		<div>
			<input type="checkbox" id="nitro" bind:checked={opt.nitroEmoji} />
			<label for="nitro">Use nitro emojis</label>
		</div>
		<div>
			<input type="checkbox" id="unkown" bind:checked={opt.unkown} />
			<label for="unkown">Convert unsupported chars to unkown</label>
		</div>
	</div>
	<div class="buttons">
		<button on:click={() => data.input = ''}>Clear</button>
	</div>
	<div class="input">
		<textarea bind:value={data.input}></textarea>
		<textarea bind:value={data.output}></textarea>
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