<script lang="ts">
	import { onMount } from 'svelte';
	import type { BeforeInstallPromptEvent } from '../models';

	import SearchSvg from '$lib/svgs/search.svelte';
	import BookmarksSvg from '$lib/svgs/bookmarks.svelte';
	import DownloadSvg from './svgs/download.svelte';

	let deferredPrompt: BeforeInstallPromptEvent | null = null;
	let showInstallButton = false;

	const install = async () => {
		if (deferredPrompt) {
			deferredPrompt.prompt();
			const { outcome } = await deferredPrompt.userChoice;
			if (outcome === 'accepted') {
				deferredPrompt = null;
			}
		}
	};

	const showInstall = () => {
		return deferredPrompt != null && !alreadyInstalled();
	};

	const alreadyInstalled = () => {
		return window.matchMedia('(display-mode: standalone)').matches;
	};

	onMount(() => {
		window.addEventListener('beforeinstallprompt', (e) => {
			deferredPrompt = e;
		});
		showInstallButton = showInstall();
	});
</script>

<nav class="sticky top-0 z-10 p-2 bg-white md:p-4">
	<div class="max-w-7xl mx-auto">
		<ul class="flex flex-row justify-end gap-4 p-2 text-sm font-medium text-gray-600">
			<li>
				<a class="flex flex-row items-center gap-1 hover:underline" href="/">
					<SearchSvg />
					Search
				</a>
			</li>
			<li>
				<a class="flex flex-row items-center gap-1 hover:underline" href="/bookmarks">
					<BookmarksSvg />
					Bookmarks
				</a>
			</li>
			{#if showInstallButton}
				<li>
					<button
						aria-label="Install App"
						class="flex flex-row gap-1 items-center font-medium hover:underline"
						on:click={install}
					>
						<DownloadSvg />
						Install</button
					>
				</li>
			{/if}
			<!-- <li>
				<div class="flex flex-col">
					<a class="flex flex-row items-center gap-1 text-gray-300 hover:underline" href="">
						<BooksSvg />
						Hadith Books
					</a>
					<p class="text-xs text-gray-300">coming soon*</p>
				</div>
			</li> -->
		</ul>
	</div>
</nav>
