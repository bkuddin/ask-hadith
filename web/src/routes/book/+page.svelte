<script lang="ts">
	import { fade } from 'svelte/transition';
	import { searchKey } from '../../store';
	import Hadith from '$lib/Hadith.svelte';
	import type { HadithModel } from '../../models';

	export let data: { hadith: HadithModel; searchKey: string };

	let hadith: HadithModel = data.hadith;
	$searchKey = data.searchKey || '';

	const ogTitle = () => {
		let text =
			hadith.collection + ' (Book: ' + hadith.book_no + ', Hadith: ' + hadith.book_ref_no + ') ';
		if (hadith.hadith_no) {
			text += 'Hadith No: ' + hadith.hadith_no;
		}
		return text;
	};

	const ogDescription = () => {
		let text = '';
		if (hadith.hadith_no) {
			text += hadith.narrator_en;
		}
		text += ' ' + hadith.body_en.substring(0, 100) + '...';
		return text;
	};
</script>

<svelte:head>
	<title>{ogTitle()}</title>
	<meta name="description" content={ogDescription()} />

	<!-- Facebook Meta Tags -->
	<meta property="og:type" content="website" />
	<meta property="og:title" content={ogTitle()} />
	<meta property="og:description" content={ogDescription()} />
	<meta property="og:image" content="https://www.askhadith.com/api/og?hadith={hadith.base64}" />

	<!-- Twitter Meta Tags -->
	<meta name="twitter:card" content="summary_large_image" />
	<meta property="twitter:domain" content="askhadith.com" />
	<meta name="twitter:title" content={ogTitle()} />
	<meta name="twitter:description" content={ogDescription()} />
	<meta name="twitter:image" content="https://www.askhadith.com/api/og?hadith={hadith.base64}" />
</svelte:head>

<div in:fade class="max-w-4xl mx-auto">
	{#if !hadith}
		<p class="flex items-center justify-center py-20">Nothing found!</p>
	{:else}
		<div class="flex flex-col">
			<div class="mx-auto">
				<Hadith bind:hadith />
			</div>
		</div>
	{/if}
	{#if $searchKey}
		<div class="flex items-center justify-center mt-10 mb-20 font-normal underline">
			<a
				href="/?search={$searchKey}"
				class="text-sm text-blue-700 hover:text-blue-500 hover:underline"
			>
				See more hadiths about "{$searchKey}"
			</a>
		</div>
	{/if}
</div>
