<script>
    import { onMount } from "svelte";
    import { spring } from 'svelte/motion';

    onMount(() => {
        document.onmousemove = (e) => {
            coords1.set({ x: e.clientX, y: e.clientY });
            coords2.set({ x: e.clientX, y: e.clientY });
            if(e.target.closest("a")) size.set(20); else size.set(10);
        }
        document.onmouseenter = () => cursor.style.opacity = `1`;
        document.onmouseleave = () => cursor.style.opacity = `0`;
        document.onmousedown = () => size.set(30);
        document.onmouseup = () => size.set(10);
    });

	let coords1 = spring(
		{ x: -10, y: -10 },
		{
			stiffness: 0.03,
			damping: 0.25
		}
	);

	let coords2 = spring(
		{ x: -10, y: -10 },
		{
			stiffness: 0.3,
			damping: 0.35
		}
	);

	let size = spring(10);
    let cursor;

    const cards = [
        {
            title:"Angus",
            body:"My portfolio",
            preview:"//angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Portfolio",
            circleClasses:"bg-[rgb(70,148,83)] right-5 top-10 group-hover:translate-y-1/2 group-hover:-translate-x-1/2"
        },
        {
            title:"Shop",
            body:"My shop project",
            preview:"//shop.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Shop",
            circleClasses:"bg-[rgb(69,43,26)] right-5 bottom-10 group-hover:-translate-y-1/2 group-hover:-translate-x-1/2"
        },
        {
            title:"FileHub",
            body:"Transfer files",
            preview:"//filehub.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/FileHub",
            circleClasses:"bg-[rgb(134,239,172)] left-5 top-10 group-hover:translate-y-1/2 group-hover:translate-x-1/2"
        },
        {
            title:"Link Trim",
            body:"Shorten links",
            preview:"//linktrim.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/linkTrim",
            circleClasses:"bg-[rgb(25,154,70)] left-5 bottom-10 group-hover:-translate-y-1/2 group-hover:translate-x-1/2"
        },
        {
            title:"Sign It Now",
            body:"Sign petitions",
            preview:"//signitnow.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/SignItNow",
            circleClasses:"bg-[rgb(26,86,219)] left-5 top-10 group-hover:translate-y-1/2 group-hover:translate-x-1/2"
        },
        {
            title:"Code Chronicles",
            body:"A blog where you can learn web development",
            preview:"//blog.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Code-Chronicles",
            circleClasses:"bg-[rgb(235,79,39)] right-5 top-10 group-hover:translate-y-1/2 group-hover:-translate-x-1/2"
        }
    ];
</script>

<svg bind:this={cursor} class="w-full h-full z-50 transition-all fixed pointer-events-none">
    <circle cx={$coords1.x} cy={$coords1.y} r={$size} stroke="white" stroke-width="1" fill-opacity="0"/>
    <circle cx={$coords2.x} cy={$coords2.y} r={$size/4} fill="white"/>
</svg>

<main class="min-h-screen flex flex-col w-full z-10">
    <div class="w-full max-w-screen-xl mx-auto px-4 md:px-6 py-24 grow items-center justify-center grid gap-6 lg:grid-cols-3">
        {#each cards as card}
            <div class="relative group h-full w-full">
                <span class="w-1/2 aspect-square absolute {card.circleClasses} rounded-full transition-all duration-1000"></span>
                <div class="bg-[rgba(65,65,65,0.308)] border border-[rgba(255,255,255,0.089)] relative w-full h-full flex flex-col justify-between items-center py-5 px-3 rounded-lg backdrop-blur-[30px]">
                    <img src="https://cruip-tutorials.vercel.app/spotlight-effect/card-01.png" width="200" height="200" alt="Card 01" />
                    <div class="grow mb-5 w-full text-center px-10">
                        <h2 class="text-2xl text-white font-bold">{card.title}</h2>
                        <p>{card.body}</p>
                    </div>

                    <div class="flex flex-row gap-2 flex-wrap w-full items-center justify-center">
                        {#if card.preview}
                            <a target="_blank" class="button" href="{card.preview}">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M13.5 6H5.25A2.25 2.25 0 003 8.25v10.5A2.25 2.25 0 005.25 21h10.5A2.25 2.25 0 0018 18.75V10.5m-10.5 6L21 3m0 0h-5.25M21 3v5.25" /></svg>
                                <span>Preview</span>
                            </a>
                        {/if}
                        
                        {#if card.code}
                            <a target="_blank" class="button" href="{card.code}">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M17.25 6.75L22.5 12l-5.25 5.25m-10.5 0L1.5 12l5.25-5.25m7.5-3l-4.5 16.5" /></svg>
                                <span>Code</span>
                            </a>
                        {/if}
                    </div>
                </div>
            </div>
        {/each}
    </div>

    <footer class="w-full border-t border-[rgba(255,255,255,0.089)] px-4 py-2 text-center">
      <p id="footer">Angus PAILLAUGUE 2023 - {new Date().getFullYear()}</p>
    </footer>
</main>