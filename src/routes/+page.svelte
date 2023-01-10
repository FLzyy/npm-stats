<script lang="ts">
	import { goto } from '$app/navigation';

	let link = '';

	const isValid = (value: string) => {
		return /^(@[a-z0-9-~][a-z0-9-._~]*\/)?[a-z0-9-~][a-z0-9-._~]*\\[~^]?([\dvx*]+(?:[-.](?:[\dx*]+|alpha|beta))*$)/g.test(
			value
		);
	};

	const onClick = () => {
		if (link && isValid(link)) {
			goto(`stats/${link.replaceAll('/', '|').replaceAll('\\', '|')}`);
		} else {
			alert('Empty or invalid package name.');
		}
	};
</script>

<main>
	<h1>Welcome to npm-stats</h1>

	<form method="POST">
		<label
			>NPM Package Name: <input
				aria-label="NPM Package Name here"
				type="text"
				id="pkgname"
				name="pkgname"
				bind:value={link}
			/></label
		>
		<button on:click|preventDefault|once={onClick}>Go!</button>
	</form>
</main>

<style>
	h1 {
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		font-weight: 600;
	}

	input,
	button {
		color: black;
	}

	form {
		text-align: center;
	}

	label {
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		font-weight: 500;
	}
</style>
