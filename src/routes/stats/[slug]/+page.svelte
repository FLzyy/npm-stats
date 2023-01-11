<script lang="ts">
	import { page } from '$app/stores';

	const getData = async (pkg: string) => {
		const response = await fetch(`https://registry.npmjs.org/${pkg.replaceAll('|', '/')}`);

		if (response.ok) {
			return await response.json();
		} else {
			throw new Error(`${await response.text()}`);
		}
	};

	let promise = getData($page.params.slug);

	const refresh = () => {
		promise = getData($page.params.slug);
	};
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<svg
	class="r-icon"
	xmlns="http://www.w3.org/2000/svg"
	viewBox="0 0 16 16"
	width="24"
	height="24"
	on:click={refresh}
	><path
		d="M5.029 2.217a6.5 6.5 0 0 1 9.437 5.11.75.75 0 1 0 1.492-.154 8 8 0 0 0-14.315-4.03L.427 1.927A.25.25 0 0 0 0 2.104V5.75A.25.25 0 0 0 .25 6h3.646a.25.25 0 0 0 .177-.427L2.715 4.215a6.491 6.491 0 0 1 2.314-1.998ZM1.262 8.169a.75.75 0 0 0-1.22.658 8.001 8.001 0 0 0 14.315 4.03l1.216 1.216a.25.25 0 0 0 .427-.177V10.25a.25.25 0 0 0-.25-.25h-3.646a.25.25 0 0 0-.177.427l1.358 1.358a6.501 6.501 0 0 1-11.751-3.11.75.75 0 0 0-.272-.506Z"
	/><path d="M9.06 9.06a1.5 1.5 0 1 1-2.12-2.12 1.5 1.5 0 0 1 2.12 2.12Z" /></svg
>

<a href="/">
	<svg class="b-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="24" height="24"
		><path
			d="M7.78 12.53a.75.75 0 0 1-1.06 0L2.47 8.28a.75.75 0 0 1 0-1.06l4.25-4.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042L4.81 7h7.44a.75.75 0 0 1 0 1.5H4.81l2.97 2.97a.75.75 0 0 1 0 1.06Z"
		/></svg
	>
</a>

<main>
	{#await promise}
		<div class="lds-ripple">
			<div />
			<div />
		</div>
	{:then data}
		<header>
			<h2>{data.name}</h2>
			<p>Description: {data.description}</p>
		</header>
		<p>Version: {data.version}</p>
	{:catch error}
		<h2>Something went wrong: {error.message}</h2>
	{/await}
</main>

<style>
	* {
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
	}

	.r-icon {
		position: fixed;
		top: 0;
		right: 0;
		fill: white;
		margin-top: 2rem;
		margin-right: 2rem;
		transition-property: transform;
		transition-timing-function: ease-out;
		transition-duration: 170ms;
		cursor: pointer;
	}

	.r-icon:hover {
		transform: scale(110%);
	}

	.b-icon {
		position: fixed;
		bottom: 0;
		left: 0;
		fill: white;
		margin-bottom: 2rem;
		margin-left: 2rem;
		transition-property: transform;
		transition-timing-function: ease-out;
		transition-duration: 170ms;
		cursor: pointer;
	}

	.b-icon:hover {
		transform: scale(110%);
	}

	.lds-ripple {
		display: inline-block;
		position: relative;
		width: 80px;
		height: 80px;
	}
	.lds-ripple div {
		position: absolute;
		border: 4px solid #fff;
		opacity: 1;
		border-radius: 50%;
		animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
	}
	.lds-ripple div:nth-child(2) {
		animation-delay: -0.5s;
	}
	@keyframes lds-ripple {
		0% {
			top: 36px;
			left: 36px;
			width: 0;
			height: 0;
			opacity: 0;
		}
		4.9% {
			top: 36px;
			left: 36px;
			width: 0;
			height: 0;
			opacity: 0;
		}
		5% {
			top: 36px;
			left: 36px;
			width: 0;
			height: 0;
			opacity: 1;
		}
		100% {
			top: 0px;
			left: 0px;
			width: 72px;
			height: 72px;
			opacity: 0;
		}
	}
</style>
