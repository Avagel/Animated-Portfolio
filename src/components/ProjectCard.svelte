<script lang="ts">
	import Phone from './Phone.svelte';

	// import PhoneII from './PhoneII.svelte';

	interface Props {
		title: string;
		description: string;
		git_link: string;
		live_link: string;
		tech_stack: string;
		ref?: (el: HTMLDivElement) => void;
		zIndex: number;
		img: string;
	}

	let { title, description, ref, git_link, tech_stack, live_link, zIndex, img }: Props = $props();

	let horizontal =
		!title.toLowerCase().includes('manhwa') && !title.toLowerCase().includes('habit');
</script>

<div
	use:ref
	class="
		flex-colitems-start absolute flex h-fit max-h-[420px] w-[90vw] shrink-0
		items-center gap-4 overflow-hidden rounded-3xl
		bg-white/5 p-6 backdrop-blur-2xl
		sm:h-[60vw] sm:max-h-[480px] sm:w-[80vw]
		lg:h-[500px] lg:max-h-none lg:w-[75vw] lg:max-w-[1200px] lg:flex-row
		lg:items-center lg:justify-center lg:gap-6 lg:pl-16
	"
	style="z-index: {zIndex};"
>
	<img
		src={img}
		class="absolute top-0 right-0 h-full w-full object-cover opacity-30 blur-2xl"
		alt=""
	/>
	<img
		src={img}
		class="absolute top-0 left-0 h-full w-full object-cover opacity-10 blur-2xl"
		alt=""
	/>

	<div class="content z-1 w-full lg:max-w-[50%]">
		<header class="mb-1 text-xs text-white/50 md:text-sm">{tech_stack}</header>
		<h1 class="text-2xl font-bold md:text-3xl lg:text-4xl">{title}</h1>
		<p class="my-3 line-clamp-3 text-sm text-white/50 md:my-6 md:text-base lg:line-clamp-none">
			{description}
		</p>
		<div class="mt-4 flex gap-3 md:mt-6 md:gap-4">
			<a href={git_link} target="_blank" class="primary-btn text-sm md:text-base">
				<span>View in Git</span>
				<div class="glow"></div>
			</a>
			{#if live_link}
				<a href={live_link} target="_blank" class="secondary-btn text-sm md:text-base">
					<span>See Live</span>
				</a>
			{/if}
		</div>
	</div>

	<!-- Phone hidden on small screens -->
	<div class="hidden h-full overflow-hidden pt-16 lg:block">
		<Phone {img} {horizontal} />
	</div>
</div>
