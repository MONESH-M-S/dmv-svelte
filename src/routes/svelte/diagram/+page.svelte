<script lang="ts">
	import { onMount } from 'svelte';
	import DbVisualizer from '../../../component/DBVisualizer.svelte';
	import { page } from '$app/stores';

	let userName: string | null = '';
	let rowData: any = {};

	onMount(async () => {
		const name = window.localStorage.getItem('userName');
		userName = name ? name : '';

		const selectedIndex = $page.url.searchParams.get('selectedIndex');

		const res = await fetch('https://www.mockachino.com/4a0c8950-04b0-4b/api/table-data');
		const data = await res.json();
		rowData = data.result.find((row: any) => {
			if (row.id == selectedIndex) return row;
		});
		console.log(rowData);
	});

	$: userName, updateUserName();

	const updateUserName = () => {
		if (userName) window.localStorage.setItem('userName', userName);
	};
</script>

<main>
	<div class="user-detail">
		<h2>Hello,</h2>
		<input type="text" bind:value={userName} />
	</div>
	<div class="row-data">
		<ul>
			{#each Object.keys(rowData) as data}
				<li>{data}: {rowData[data]}</li>
			{/each}
		</ul>
	</div>
	<div class="object-data" />
	<div class="db-visual">
		<DbVisualizer />
	</div>
</main>

<style>
	main {
		height: 90vh;
		width: 90vw;
		margin: 0 auto;

		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.user-detail {
		display: flex;
		align-items: center;
		gap: 1rem;
	}

	.user-detail input {
		height: 30px;
	}

	.row-data {
		flex-grow: 2;
		box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
	}

	.row-data li {
		margin-top: 0.5rem;
	}

	.db-visual {
		flex-grow: 7;
		padding: 2rem;
		border: 1px solid #000;
	}
</style>
