<script lang="ts">
	import { page } from '$app/stores';

	const getData = async (pkg: string) => {
		const response = await fetch(`https://registry.npmjs.org/${pkg.replaceAll('|', '/')}`);
		const json = response.json();

		if (response.ok) {
			return json;
		} else {
			console.error(json);
		}
	};

	const promise = getData($page.params.slug);
</script>

{#await promise}
	<p>We're getting your data...</p>
{:then data}
	<p>{data.name}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
