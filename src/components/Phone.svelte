<script lang="ts">
	let { img, horizontal } = $props();
</script>

{#if horizontal}
	<div class="relative w-158">
		<img class="h-auto w-full rotate-[90] object-cover" src="/ipad.png" alt="" />
		<img
			class="absolute inset-0 top-[50%] left-[50%] h-[97%] w-[93%] translate-x-[-49.5%] translate-y-[-50%] rounded-[45px] bg-zinc-950 object-cover"
			src={img}
			alt=""
		/>
	</div>
{:else}
	<div class="relative h-158 w-80">
		<img class="h-auto w-full object-cover" src="/phone.png" alt="" />
		<img
			class="absolute inset-0 top-[50%] left-[50%] h-[97%] w-[93%] translate-x-[-49.5%] translate-y-[-50%] rounded-[45px] bg-zinc-950 object-cover"
			src={img}
			alt=""
		/>
		<div
			class="absolute inset-0 top-6 left-[50%] h-8 w-[40%] translate-x-[-50%] rounded-full bg-zinc-950"
		></div>
	</div>
{/if}
