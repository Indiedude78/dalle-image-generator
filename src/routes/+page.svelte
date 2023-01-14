<script>
	import { Configuration, OpenAIApi } from "openai";

	const config = new Configuration({
		organization: import.meta.env.VITE_OPENAI_ORG_ID,
		apiKey: import.meta.env.VITE_OPENAI_API_KEY,
	});
	const openai = new OpenAIApi(config);

	let prompt = "";

	async function getAIImages({ prompt, numOfImages, size }) {
		const imagesResponse = await openai.createImage({
			prompt: prompt,
			n: numOfImages,
			size: size,
		});
		const imageData = await imagesResponse.data;
		return imageData;
	}

	let isGenerating = false;
	let imageURL = "";
	const handleClick = async (prompt) => {
		console.log(prompt);
		isGenerating = true;
		const image = await getAIImages({
			prompt: prompt,
			numOfImages: 1,
			size: "512x512",
		});
		console.log(image.data[0].url);
		imageURL = await image.data[0].url;
		prompt = "";
		isGenerating = false;
	};
</script>

<h1>Ai Art</h1>

<div class="img-form-container">
	<img
		src="./../src/assets/images/ai.jpg"
		alt="Ai neural connection"
		class="ai-img"
	/>
	<div class="form-container">
		<input
			type="text"
			name="prompt"
			id="prompt"
			placeholder="Enter a prompt to start..."
			bind:value={prompt}
		/>
		<button on:click={handleClick(prompt)}>Generate</button>
	</div>
</div>
<div>
	{#if isGenerating}
		<div class="loading-container">
			<div id="loader-one" />
			<div id="loader-two" />
			<div id="loader-three" />
			<div id="loader-four" />
		</div>
	{:else}
		<div class="generated-image-container">
			<h3>Generated</h3>
			<img src={imageURL} alt="" />
		</div>
	{/if}
</div>

<style>
	h1 {
		letter-spacing: 1rem;
		text-align: center;
		margin: 2rem 0;
	}

	.img-form-container {
		display: flex;
		justify-content: center;
		align-items: center;
		margin: 2rem auto;
		width: 75%;
		position: relative;
	}

	.img-form-container img {
		width: min(100%, 500px);
		border-radius: 10px;
		filter: opacity(0.5);
		-webkit-filter: opacity(0.5);
		object-fit: contain;
		margin: auto;
	}

	.img-form-container .form-container {
		position: absolute;
		bottom: -15px;
		border-radius: 5px;
		background-color: transparent;
	}

	:is(.form-container input, .form-container button) {
		padding: 0.5rem 1rem;
		border: none;
		border-radius: 5px;
		background-color: hsla(0, 0%, 50%, 0.159);
		outline: none;
	}

	.form-container button:hover {
		cursor: pointer;
		background-color: hsla(149, 26%, 51%, 0.416);
	}

	.generated-image-container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 2rem auto;
		gap: 2rem;
		width: 100%;
		position: relative;
	}

	.generated-image-container h3 {
		letter-spacing: 1ch;
		text-transform: uppercase;
	}

	.loading-container {
		margin-top: 5rem;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		gap: 1rem;
	}

	.loading-container div {
		background-color: white;
		height: 20px;
		width: 20px;
		border-radius: 20px;
	}

	@keyframes loading {
		0% {
			transform: translateY(20px) scale(1);
		}
		50% {
			transform: translateY(-20px) scale(1.5);
		}
		100% {
			transform: translateY(20px) scale(1);
		}
	}

	#loader-one {
		animation: loading 1s infinite ease-in-out forwards;
	}

	#loader-two {
		animation: loading 1s infinite ease-in-out forwards;
		animation-delay: 0.3s;
	}

	#loader-three {
		animation: loading 1s infinite ease-in-out forwards;
		animation-delay: 0.6s;
	}

	#loader-four {
		animation: loading 1s infinite ease-in-out forwards;
		animation-delay: 0.9s;
	}
</style>
