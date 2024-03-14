<script>

	import Login from '$lib/Login.svelte';

	import { onMount } from 'svelte';

	import { theme } from '../store/theme';


	// Post variables
	/** @type {string}*/
	let postLink = 'https://t.me/computly/159';
	/** @type {string}*/
	let postColor = '#2f81f6';
	/** @type {string}*/
	let postColorDark = '#89baff';
	/** @type {boolean}*/
	let darkThemeCheckbox = false;



	// Login variables
	/** @type {string}*/
	let loginUsername = 'RTverBot';

	async function telegramLogin(/** @type any*/ data) {
		/** @type {import('../lib/types/user').user}*/
		const user = data.detail;
		const res = await fetch(window.location.origin + '/bot', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				id: user.id
			})
		});
		console.log(await res.json());
	}

	function switchTheme() {
		theme.set(darkThemeCheckbox ? 'dark' : 'light');
		darkThemeCheckbox
			? document.body.classList.add('dark')
			: document.body.classList.remove('dark');
	}
	onMount(() => {
		darkThemeCheckbox = $theme === 'dark';
		switchTheme();
	});
</script>

<svelte:head>
	<title>Sveltegram</title>
</svelte:head>



<div class="widget">
	<Login username={loginUsername} requestAccess={true} on:auth={telegramLogin} />
</div>

<style>
	.controls {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		place-content: center;
		place-items: start;
		align-items: center;
		gap: 2ch;
		margin-bottom: 1rem;
	}
	.controls input[type='text'] {
		padding: 0.5em 1em;
		/* border-radius: 5px;
		border: none;
		outline: none; */
	}
	.controls input[type='checkbox'] {
		width: 1.5rem;
		height: 1.5rem;
	}
	.controls input[type='color'] {
		width: 2rem;
		height: 2rem;
		border: none;
		outline: none;
	}

	pre {
		border-radius: 10px;
		line-height: 1.5;
		font-size: 1em;
	}
	.code-demo {
		height: 250px;
		overflow: auto;
	}

	.widget {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 1rem;
		margin-bottom: 1rem;
	}
</style>
