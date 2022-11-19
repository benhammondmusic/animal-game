<script lang="ts">

	async function animalImageUrl(): Promise<string[]> {
		const response = await fetch(`https://zoo-animal-api.herokuapp.com/animals/rand`);
		const json = await response.json();
		return [json.name, json.image_link];
	}

	let promise = animalImageUrl()
	const handleClick = () => promise = animalImageUrl()
</script>

<main>
<h1>Animals!!</h1>

<button on:click={handleClick}>
	New Animal
	</button>

<span class="animal-box">
	{#await promise}
		<small>loading...</small>
	{:then [name, url]}
	<figure>
		<figcaption>{name}</figcaption>
		<img class="animal-pic" alt={name} src={url} />
	</figure>
	{/await}
</span>



</main>


<style>

	@font-face {
			font-family: 'Gelasio';
			font-style: normal;
			font-weight: 400;
			src: local('Gelasio Regular'), local('Gelasio-Regular'), url(https://fonts.gstatic.com/s/gelasio/v1/cIf9MaFfvUQxTTqS9C6hYQ.woff2) format('woff2');
			unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
	}

	* {
		font-family: Gelasio

	}

	h1 {
			font-size: 5rem;
	}

	button {
		font-size: 3rem;
		display: inline;
	}
	button:hover {
			cursor: pointer;
	}
	figure {
		display: inline;
	}
	figcaption {
		font-size: 3rem;
	}
	.animal-pic {
	width: 200px;
	font-size: 2rem;

	}
</style>