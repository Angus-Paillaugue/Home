<script>
    import { onMount } from "svelte";
    import { spring } from 'svelte/motion';


    onMount(() => {
        const app = document.querySelector("#app");

        new Spotlight(app);

        window.onmousemove = (e) => {
            coords1.set({ x: e.clientX, y: e.clientY });
            coords2.set({ x: e.clientX, y: e.clientY });
        }

        window.onmouseenter = (e) => {
            coords1.set({ x: e.clientX, y: e.clientY });
            coords2.set({ x: e.clientX, y: e.clientY });
        }
        document.onmouseleave = (e) => {
            coords1.set({ x: e.clientX, y: e.clientY-20 });
            coords2.set({ x: e.clientX, y: e.clientY-20 });
        }
    });

	let coords1 = spring(
		{ x: 0, y: 0 },
		{
			stiffness: 0.05,
			damping: 0.25
		}
	);

	let coords2 = spring(
		{ x: 0, y: 0 },
		{
			stiffness: 0.1,
			damping: 0.35
		}
	);

	let size = spring(10);


    class Spotlight {
        constructor(containerElement) {
            this.container = containerElement;
            this.cards = Array.from(this.container.children);
            this.mouse = {
                x: 0,
                y: 0,
            };
            this.containerSize = {
                w: 0,
                h: 0,
            };
            this.initContainer = this.initContainer.bind(this);
            this.onMouseMove = this.onMouseMove.bind(this);
            this.init();
        }

        initContainer() {
            this.containerSize.w = this.container.offsetWidth;
            this.containerSize.h = this.container.offsetHeight;
        }

        onMouseMove(event) {
            const { clientX, clientY } = event;
            const rect = this.container.getBoundingClientRect();
            const { w, h } = this.containerSize;
            const x = clientX - rect.left;
            const y = clientY - rect.top;
            const inside = x < w && x > 0 && y < h && y > 0;
            if (inside) {
                this.mouse.x = x;
                this.mouse.y = y;
                this.cards.forEach((card) => {
                    const cardX = -(card.getBoundingClientRect().left - rect.left) + this.mouse.x;
                    const cardY = -(card.getBoundingClientRect().top - rect.top) + this.mouse.y;
                    card.style.setProperty('--mouse-x', `${cardX}px`);
                    card.style.setProperty('--mouse-y', `${cardY}px`);
                });
            }
        }

        init() {
            this.initContainer();
            window.addEventListener('resize', this.initContainer);
            window.addEventListener('mousemove', this.onMouseMove);
        }
    }

    const cards = [
        {
            title:"Angus",
            body:"My portfolio",
            preview:"//angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Portfolio"
        },
        {
            title:"Shop",
            body:"My shop project",
            preview:"//shop.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Shop"
        },
        {
            title:"FileHub",
            body:"Transfer files",
            preview:"//filehub.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/FileHub"
        },
        {
            title:"Link Trim",
            body:"Shorten links",
            preview:"//linktrim.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/linkTrim"
        },
        {
            title:"Sign It Now",
            body:"Sign petitions",
            preview:"//signitnow.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/SignItNow"
        },
        {
            title:"Code Chronicles",
            body:"A blog where you can learn web development",
            preview:"//blog.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Code-Chronicles"
        }
    ];
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<svg
	on:mousedown={(e) => {
		size.set(30);
		e.stopPropagation();
	}}
	on:mouseup={(e) => {
		size.set(10);
		e.stopPropagation();
    }}
	class ="w-full h-full z-50 fixed pointer-events-none"
>
	<circle cx={$coords1.x} cy={$coords1.y} r={$size} stroke="lightgray" stroke-width="1" fill-opacity="0"/>
	<circle cx={$coords2.x} cy={$coords2.y} r={$size/4} fill="darkgray"/>
</svg>

<main class="min-h-screen flex flex-col w-full">
    <div class="w-full max-w-6xl mx-auto px-4 md:px-6 py-24 grow flex flex-col items-center justify-center">
        <div class="max-w-sm mx-auto grid gap-6 lg:grid-cols-3 items-start lg:max-w-none group w-full" id="app">
            {#each cards as card}
                <div class="card">
                    <div class="relative h-full bg-slate-900 p-6 pb-8 rounded-[inherit] z-20 overflow-hidden">
                        <div class="absolute bottom-0 translate-y-1/2 left-1/2 -translate-x-1/2 pointer-events-none -z-10 w-1/2 aspect-square" aria-hidden="true">
                            <div class="absolute inset-0 translate-z-0 bg-slate-800 rounded-full blur-[80px]"></div>
                        </div>
                        <div class="flex flex-col h-full items-center text-center">
                            <div class="relative inline-flex">
                            <div class="w-[40%] h-[40%] absolute inset-0 m-auto -translate-y-[10%] blur-3xl -z-10 rounded-full bg-indigo-600" aria-hidden="true"></div>
                            <img class="inline-flex" src="https://cruip-tutorials.vercel.app/spotlight-effect/card-01.png" width="200" height="200" alt="Card 01" />
                            </div>
                            <div class="grow mb-5">
                                <h2>{card.title}</h2>
                                <p>{card.body}</p>
                            </div>
                
                            <div class="flex flex-row gap-2 flex-wrap w-full items-center justify-center">
                                {#if card.preview}
                                    <a target="_blank" class="button group" href="{card.preview}">
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M13.5 6H5.25A2.25 2.25 0 003 8.25v10.5A2.25 2.25 0 005.25 21h10.5A2.25 2.25 0 0018 18.75V10.5m-10.5 6L21 3m0 0h-5.25M21 3v5.25" /></svg>
                                    <span>Preview</span>
                                    </a>
                                {/if}
                                
                                {#if card.code}
                                    <a target="_blank" class="button group" href="{card.code}">
                                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M17.25 6.75L22.5 12l-5.25 5.25m-10.5 0L1.5 12l5.25-5.25m7.5-3l-4.5 16.5" /></svg>
                                        <span>Code</span>
                                    </a>
                                {/if}
                            </div>
                        </div>
                    </div>
                </div>
            {/each}
        </div>
    </div>

    <footer class="w-full border-t border-slate-700 px-4 py-2 text-center">
      <p id="footer">Angus PAILLAUGUE 2023 - {new Date().getFullYear()}</p>
    </footer>
</main>