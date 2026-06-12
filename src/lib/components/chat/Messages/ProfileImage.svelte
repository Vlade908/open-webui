<script lang="ts">
	import { WEBUI_BASE_URL } from '$lib/constants';
	import { safeImageUrl } from '$lib/utils/safeImageUrl';

	export let className = 'size-8';
	export let src = `${WEBUI_BASE_URL}/static/favicon.png`;
	export let name: string = '';

	const FALLBACK = `${WEBUI_BASE_URL}/static/favicon.png`;

	let imgError = false;

	$: initials = name
		? name
				.split(' ')
				.slice(0, 2)
				.map((w) => w[0]?.toUpperCase() ?? '')
				.join('')
		: '';

	$: bgHue = name
		? Math.abs(
				name.split('').reduce((acc, char) => acc + char.charCodeAt(0), 0) % 360
			)
		: 220;

	$: bgColor = `hsl(${bgHue}, 55%, 45%)`;

	function handleError(e: Event) {
		const target = e.currentTarget as HTMLImageElement;
		// Se já está na imagem de fallback, mostra o avatar com iniciais
		if (target.src === FALLBACK || target.src.endsWith('/static/favicon.png')) {
			imgError = true;
		} else {
			// Tenta o fallback primeiro
			target.src = FALLBACK;
		}
	}

	// Reset quando src muda
	$: {
		src;
		imgError = false;
	}
</script>

{#if !imgError}
	<img
		aria-hidden="true"
		src={safeImageUrl(src)}
		class=" {className} object-cover rounded-full"
		alt="profile"
		draggable="false"
		on:error={handleError}
	/>
{:else if initials}
	<div
		class="{className} rounded-full flex items-center justify-center text-white font-semibold select-none shrink-0"
		style="background: {bgColor}; font-size: 0.65rem;"
		aria-hidden="true"
	>
		{initials}
	</div>
{:else}
	<div
		class="{className} rounded-full flex items-center justify-center bg-gray-500 shrink-0"
		aria-hidden="true"
	>
		<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white" class="size-4">
			<path
				fill-rule="evenodd"
				d="M7.5 6a4.5 4.5 0 1 1 9 0 4.5 4.5 0 0 1-9 0ZM3.751 20.105a8.25 8.25 0 0 1 16.498 0 .75.75 0 0 1-.437.695A18.683 18.683 0 0 1 12 22.5c-2.786 0-5.433-.608-7.812-1.7a.75.75 0 0 1-.437-.695Z"
				clip-rule="evenodd"
			/>
		</svg>
	</div>
{/if}
