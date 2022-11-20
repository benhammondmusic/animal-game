<script lang="ts">

	let rotation = 0

function onKeyDown(e:any) {
		 switch(e.keyCode) {
			 case 37:
				 goRight = false
				 rotation += 10
				 break;
			 case 39:
				 goRight = true
				 rotation -= 10

				 break;
		 }
		}

let count = 0
let goRight = true
let innerWidth: number
const intervalID = setInterval(myCallback, 10);
function myCallback()
{
	let xSpeed = 3
	let increment = xSpeed * (goRight ? 1 : -1)
	count = (count + increment)


	if (count > innerWidth - 400) goRight = false
	if (count < -100) goRight = true
}

	$: x = count
	async function animalImageUrl(): Promise<string[]> {
		const response = await fetch(`https://zoo-animal-api.herokuapp.com/animals/rand`);
		const json = await response.json();
		console.log(json);
		return [json.name, json.image_link];
	}

	let promise = animalImageUrl()
	const handleClick = () => promise = animalImageUrl()
</script>

<svelte:window 
bind:innerWidth={innerWidth}
on:keydown|preventDefault={onKeyDown}
/>

<main style="--x: {x}px; --direction: {goRight ? 1: -1}; --rotation: {rotation}deg">
<!-- <h1>Animals!!</h1> -->

	{#await promise}
		<small>loading...</small>
	{:then [name, url]}
	<figure>
		<figcaption>{name}</figcaption>
		<button on:click={handleClick} class="animal-box ball">
		<img class="animal-pic" alt={name} src={url} />
		</button>
	</figure>
	{/await}
</main>

<style>
	@font-face {
			font-family: 'Gelasio';
			font-style: normal;
			font-weight: 400;
			src: local('Gelasio Regular'), local('Gelasio-Regular'), url(https://fonts.gstatic.com/s/gelasio/v1/cIf9MaFfvUQxTTqS9C6hYQ.woff2) format('woff2');
			unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
	}

	main {
		font-family: Gelasio;
		display: flex;

	}

	button {
		font-size: 2rem;
		background-color: none;
		border: none;
		background: none;
	}
	button:hover {
			cursor: pointer;
	}
	figcaption {
		font-size: 3rem;
	}
	.animal-pic {
	width: 400px;
	font-size: 1rem;
	border-radius: 50%;
	transform: scaleX(var(--direction));
	}



	.ball {
		max-width: 450px;
		max-height: 450px;
		border-radius: 50%;
		background-color: #FF5722;

		animation: bounce 1.5s;
		animation-direction: alternate;
		animation-timing-function: cubic-bezier(.5,0.05,1,.5);
		animation-iteration-count: infinite;
	}

	@keyframes bounce {
		from { transform: rotate(var(--rotation)) translate3d(calc(var(--x) + 2vh),0, 0);     }
		to   { transform: rotate(var(--rotation)) translate3d(var(--x), 60vh, 0); }
	}
</style>