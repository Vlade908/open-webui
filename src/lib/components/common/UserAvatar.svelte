<script lang="ts">
	export let src: string = '';
	export let name: string = '';
	export let size: string = 'size-7';
	export let className: string = '';
	export let fontSize: string = '0.65rem';

	let imgError = false;

	$: initials = name
		? name
				.split(' ')
				.slice(0, 2)
				.map((w) => w[0]?.toUpperCase() ?? '')
				.join('')
		: '?';

	// Gera uma cor de fundo consistente baseada no nome do usuário
	$: bgHue = name
		? Math.abs(
				name.split('').reduce((acc, char) => {
					return acc + char.charCodeAt(0);
				}, 0) % 360
			)
		: 220;

	$: bgColor = `hsl(${bgHue}, 55%, 45%)`;

	function handleError() {
		imgError = true;
	}

	// Reset quando o src muda
	$: {
		src;
		imgError = false;
	}
</script>

{#if !imgError && src}
	<img
		{src}
		class="{size} {className} object-cover rounded-full"
		alt={name || 'User avatar'}
		on:error={handleError}
	/>
{:else}
	<div
		class="{size} {className} rounded-full flex items-center justify-center text-white font-semibold select-none shrink-0"
		style="background: {bgColor}; font-size: {fontSize};"
		title={name}
	>
		{initials}
	</div>
{/if}
