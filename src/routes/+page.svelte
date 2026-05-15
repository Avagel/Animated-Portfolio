<script lang="ts">
	import Contacts from '../components/Contacts.svelte';
	import Loader from '../components/Loader.svelte';
	import ProjectCard from '../components/ProjectCard.svelte';
	import Test from '../components/Test.svelte';
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';
	import { SplitText } from 'gsap/SplitText';
	import { dummyProjects } from '../data/projects.ts';
	import { testimonials } from '../data/testimonial.ts';
	import AOS from 'aos';
	import 'aos/dist/aos.css';

	let hero: HTMLElement;
	let subtitle: HTMLElement;
	let name: HTMLElement;
	let circle: HTMLElement;
	let stat1: HTMLElement;
	let stat2: HTMLElement;
	let stat3: HTMLElement;
	let heroInner: HTMLElement;
	let about: HTMLElement;
	let testimonial: HTMLElement;
	let testimonialText: HTMLElement;
	let projectCard: HTMLElement;
	let testCards: HTMLElement;
	let projectText: HTMLElement;
	let project: HTMLElement;
	let projectCards: HTMLElement[] = [];
	let projectsContainer: HTMLElement;

	gsap.registerPlugin(ScrollTrigger, SplitText);

	onMount(() => {
		AOS.init({ easing: 'ease-out', once: false });

		const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });
		tl.from(subtitle, { y: 20, opacity: 0, duration: 1 })
			.from(name, { y: 60, opacity: 0, duration: 1 }, '-=0.5')
			.from(circle, { scale: 0.5, opacity: 0, duration: 1.5, ease: 'power2.out' }, '-=0.8')
			.from([stat1, stat2, stat3], { y: 30, opacity: 0, duration: 0.8, stagger: 0.15 }, '-=0.6');

		const split = SplitText.create('.about-text', { type: 'words' });

		gsap.to(heroInner, {
			rotateZ: -30,
			rotateX: 30,
			filter: 'blur(30px)',
			borderRadius: '50px',
			opacity: 0,
			scale: 0.5,
			ease: 'none',
			scrollTrigger: {
				trigger: heroInner,
				start: 'top top',
				endTrigger: about,
				end: 'top top',
				scrub: 1
			}
		});

		gsap.to(about, {
			rotateZ: -30,
			rotateX: 30,
			filter: 'blur(30px)',
			borderRadius: '50px',
			opacity: 0,
			scale: 0.5,
			ease: 'none',
			scrollTrigger: {
				trigger: testimonialText,
				start: 'top bottom',
				endTrigger: testimonial,
				end: 'top top',
				scrub: 1
			}
		});

		gsap.to(testimonial, {
			y: '-100%',
			opacity: 0,
			ease: 'power2.in',
			scrollTrigger: { trigger: projectText, start: 'top bottom', end: 'top center', scrub: 1 }
		});

		gsap.to(project, {
			y: '-100%',
			opacity: 0,
			ease: 'power2.in',
			scrollTrigger: {
				trigger: projectsContainer,
				start: 'top bottom',
				end: 'top center',
				scrub: 1
			}
		});

		gsap.to(split.words, {
			color: '#fff',
			stagger: 0.1,
			scrollTrigger: { trigger: '.about-text', start: 'top center', end: 'top top', scrub: true }
		});

		projectCards.forEach((card, i) => {
			const total = projectCards.length;
			const start = `${(i / total) * 100}%`;
			const end = `${((i + 1) / total) * 100}%`;

			gsap.to(card, {
				y: '-110%',
				ease: 'power2.in',
				scrollTrigger: { trigger: projectsContainer, start, end, scrub: 1 }
			});

			if (i > 0) {
				gsap.from(card, {
					scale: 1.3,
					opacity: 0,
					ease: 'power2.out',
					scrollTrigger: {
						trigger: projectsContainer,
						start: `${((i - 1) / total) * 100}%`,
						end: start,
						scrub: 1
					}
				});
			}
		});
	});
</script>

<nav
	class="fixed top-4 z-50 flex items-center gap-3 rounded-3xl border border-white/5 bg-white/9 px-4 py-2 text-xs shadow-[0px_0px_20px_rgba(255,255,255,0.05)] backdrop-blur-2xl md:top-6 md:gap-6 md:px-8 md:text-sm"
>
	<a class="nav-link" href="#home"><span>Home</span></a>
	<a class="nav-link" href="#about"><span>About</span></a>
	<a class="nav-link" href="#projects"><span>Works</span></a>
	<a class="nav-link" href="#contact"><span>Contact</span></a>
	<a class="nav-link hidden sm:block" href="#testimonials"><span>Testimonials</span></a>
</nav>

<div class="backg fixed inset-0 top-0 h-screen w-full bg-zinc-950"></div>
<div id="home"></div>
<div
	bind:this={heroInner}
	class="bg fade-edges sticky top-0 z-1 h-screen w-full shrink-0 overflow-hidden bg-zinc-950/50 p-6 md:p-10 xl:p-10 2xl:p-32"
>
	<div
		class="relative flex h-full w-full flex-1 shrink-0 flex-col items-center justify-center gap-6 text-center xl:items-start xl:text-left"
	>
		<div>
			<p bind:this={subtitle} class="mb-1 ml-2 text-xs tracking-widest text-white/50 md:text-sm">
				SOFTWARE ENGINEER
			</p>
			<h1
				bind:this={name}
				class=" text-4xl font-bold tracking-wide sm:text-5xl lg:text-6xl 2xl:text-7xl"
			>
				AKANUME <br /> IRUOGHENE
			</h1>
		</div>
		<div class=" xl:-0 flex-col items-center gap-2 text-xl md:flex xl:absolute xl:right-32">
			<p class="mb-1 hidden text-3xl xl:block">Download Resume</p>
			<a href="/resume.pdf" download class="resume-btn">
				<span class="xl:hidden">Download Resume</span>
				<span class="hidden xl:block"> Resume</span>
				<div class="glow"></div>
			</a>
		</div>

		<div
			bind:this={circle}
			class="circle opacity absolute bottom-0 flex h-fit w-full items-center justify-center xl:inset-0 xl:top-[50%]"
		>
			<Loader />
		</div>
	</div>

	<!-- Resume btn — hidden on mobile, shown md+ -->

	<!-- Stats — stack on mobile, row on md+ -->
	<footer
		class="bottom-15 left-0 z-4 flex h-fit w-full shrink-0 justify-center gap-6 md:right-10 md:gap-16 md:pr-10 xl:justify-end"
	>
		<div bind:this={stat1}>
			<p class="text-xl font-medium md:text-3xl">600+</p>
			<p class="font-regular text-xs text-white/50 md:text-base">Works Done</p>
		</div>
		<div bind:this={stat2}>
			<p class="text-xl font-medium md:text-3xl">600+</p>
			<p class="font-regular text-xs text-white/50 md:text-base">Works Done</p>
		</div>
		<div bind:this={stat3}>
			<p class="text-xl font-medium md:text-3xl">600+</p>
			<p class="font-regular text-xs text-white/50 md:text-base">Works Done</p>
		</div>
	</footer>
</div>

<!-- ABOUT -->
<div
	id="about"
	bind:this={about}
	class="fade-edges sticky top-0 z-2 flex h-screen w-full shrink-0 items-center justify-center p-6 px-6 md:p-10 md:px-16 xl:px-30 2xl:p-32"
>
	<div
		class="flex w-full max-w-2xl shrink-0 flex-col items-center justify-center gap-6 md:max-w-none md:gap-8 2xl:max-w-300"
	>
		<p class="about-text mt-1 w-full text-center text-lg text-white/3 md:text-2xl">
			I'm a software engineer focused on building modern digital experiences that combine
			performance, scalability, and thoughtful design. I enjoy creating interfaces that feel smooth,
			intuitive, and alive — from interactive frontend experiences to full-stack applications
			engineered for real-world use.
		</p>
		<h1
			class="w-full place-self-end text-right text-3xl font-bold tracking-wide md:w-[50%] lg:text-4xl 2xl:text-5xl"
		>
			About <span class="text-green-500"> me </span>
		</h1>
	</div>
</div>

<!-- TESTIMONIALS HEADING -->
<div
	bind:this={testimonial}
	id="testimonials"
	// bind:this={testimonial}
	class=" sticky top-0 z-2 flex h-screen w-full shrink-0 flex-col items-center justify-center px-6 2xl:p-32"
>
	<h1
		bind:this={testimonialText}
		data-aos="fade-up"
		class="text-center text-4xl font-bold tracking-wide sm:text-5xl lg:text-6xl 2xl:text-7xl"
	>
		Testimonials
	</h1>
	<p data-aos="zoom-out" class="mt-4 px-4 text-center text-base text-white/50 md:text-2xl">
		"Interactive experiences built with <span class="text-green-500"> animation </span>,
		performance, and <span class="text-green-500"> modern </span> frontend technologies."
	</p>
</div>

<div bind:this={testCards} class=" relative z-3 w-full shrink-0 2xl:p-32">
	<!-- Mobile/tablet: grid layout -->
	<div class="grid grid-cols-1 place-items-center gap-6 p-6 sm:grid-cols-2 md:p-10 lg:hidden">
		{#each testimonials as t}
			<div data-aos="zoom-out">
				<Test data={t} />
			</div>
		{/each}
	</div>

	<!-- Desktop: absolute scattered layout -->
	<div class="relative hidden h-screen w-full lg:block">
		<div data-aos="zoom-out-left" class="absolute top-[8%] right-[20%]">
			<Test data={testimonials[0]} />
		</div>
		<div data-aos="zoom-out-right" class="absolute bottom-[8%] left-[15%]">
			<Test data={testimonials[1]} />
		</div>
		<div class="absolute top-[50%] left-[50%] translate-x-[-50%] translate-y-[-50%]">
			<Test data={testimonials[2]} />
		</div>
		<div data-aos="zoom-out-left" class="absolute right-[15%] bottom-[12%]">
			<Test data={testimonials[3]} />
		</div>
		<div data-aos="zoom-out-right" class="absolute top-[8%] left-[20%]">
			<Test data={testimonials[4]} />
		</div>
	</div>

	<!-- PROJECTS HEADING -->
	<div
		id="projects"
		bind:this={project}
		class="fade-edges sticky top-0 z-2 flex h-screen w-full shrink-0 flex-col items-center justify-center gap-4 px-6 md:gap-6 2xl:p-32"
	>
		<h1
			bind:this={projectText}
			data-aos="fade-up"
			class="text-center text-4xl font-bold tracking-wide sm:text-5xl lg:text-6xl 2xl:text-7xl"
		>
			Projects
		</h1>
		<p data-aos="zoom-out" class="mt-1 px-4 text-center text-base text-white/50 md:text-2xl">
			"Interactive experiences built with <span class="text-green-500"> animation </span>,
			performance, and <span class="text-green-500"> modern </span> frontend technologies."
		</p>
	</div>

	<!-- PROJECT CARDS — stacked scroll on desktop, grid on mobile/tablet -->
	<!-- Mobile/tablet grid -->
	<div
		class="relative z-2 grid h-screen grid-cols-1 place-items-center gap-6 p-6 md:grid-cols-2 md:p-10 lg:hidden"
	>
		{#each dummyProjects as proj}
			<ProjectCard
				live_link={proj.live_link}
				title={proj.title}
				description={proj.description}
				git_link={proj.git_link}
				tech_stack={proj.tech_stack}
				zIndex={1}
				img={proj.img}
			/>
		{/each}
	</div>

	<!-- Desktop stacked scroll -->
	<div
		bind:this={projectsContainer}
		class="relative hidden lg:block"
		style="height: {dummyProjects.length * 100}vh;"
	>
		<div class="sticky top-0 flex h-screen w-full items-center justify-center">
			{#each dummyProjects as proj, i}
				<ProjectCard
					live_link={proj.live_link}
					title={proj.title}
					ref={(el) => (projectCards[i] = el)}
					description={proj.description}
					git_link={proj.git_link}
					tech_stack={proj.tech_stack}
					zIndex={dummyProjects.length - i}
					img={proj.img}
				/>
			{/each}
		</div>
	</div>

	<!-- CONTACT -->
	<div
		id="contact"
		class="fade-edges sticky top-0 z-2 flex h-screen w-full shrink-0 flex-col items-center justify-center px-6 2xl:p-32"
	>
		<h1
			data-aos="fade-up"
			class="text-center text-4xl font-bold tracking-wide sm:text-5xl lg:text-6xl 2xl:text-7xl"
		>
			Contact Me
		</h1>
		<div data-aos="zoom-out" class="mt-4 text-center text-base text-white/50 md:text-2xl">
			<Contacts />
		</div>
	</div>
</div>
