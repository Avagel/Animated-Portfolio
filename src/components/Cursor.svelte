<!-- Cursor.svelte -->
<script>
	import { onMount, onDestroy } from 'svelte';
	import gsap from 'gsap';

	let cursor;
	let follower;
	let mouseX = 0;
	let mouseY = 0;

	onMount(() => {
		// hide default cursor

		const moveCursor = (e) => {
			mouseX = e.clientX;
			mouseY = e.clientY;

			// dot snaps instantly
			gsap.to(cursor, {
				x: mouseX,
				y: mouseY,
				duration: 0
			});

			// ring follows with lag
			gsap.to(follower, {
				x: mouseX,
				y: mouseY,
				duration: 0.25,
				ease: 'power3.out'
			});
		};

		const onEnter = () => {
			gsap.to(follower, { scale: 2.5, opacity: 0.5, duration: 0.3 });
			gsap.to(cursor, { scale: 0, duration: 0.3 });
		};

		const onLeave = () => {
			gsap.to(follower, { scale: 1, opacity: 1, duration: 0.3 });
			gsap.to(cursor, { scale: 1, duration: 0.3 });
		};

		// expand on hoverable elements
		const hoverables = document.querySelectorAll('a, button, [data-cursor]');
		hoverables.forEach((el) => {
			el.addEventListener('mouseenter', onEnter);
			el.addEventListener('mouseleave', onLeave);
		});

		window.addEventListener('mousemove', moveCursor);

		return () => {
			window.removeEventListener('mousemove', moveCursor);
			document.body.style.cursor = 'auto';
			hoverables.forEach((el) => {
				el.removeEventListener('mouseenter', onEnter);
				el.removeEventListener('mouseleave', onLeave);
			});
		};
	});
</script>

<!-- dot -->
<!-- <div bind:this={cursor} class="cursor-dot" /> -->

<!-- ring -->
<div bind:this={follower} class="cursor-ring bg-green-500"></div>

<style>
	.cursor-dot {
		position: fixed;
		top: -4px;
		left: -4px;
		width: 8px;
		height: 8px;
		background: #22c55e;
		border-radius: 50%;
		pointer-events: none;
		z-index: 9999;
		will-change: transform;
	}

	.cursor-ring {
		position: fixed;
		top: -20px;
		left: -20px;
		width: 40px;
		height: 80px;
		filter: blur(60px);
		border: 1.5px solid rgba(255, 255, 255, 0.5);
		/* border-radius: 50%; */
		pointer-events: none;
		z-index: 9999;
		will-change: transform;
	}
</style>
