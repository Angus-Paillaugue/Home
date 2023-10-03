<script>
    import { onMount } from "svelte";
    import { spring } from 'svelte/motion';

    onMount(() => {
        document.onmousemove = (e) => {
            if(e.target.closest("a")) {
                let rect = e.target.closest("a").getBoundingClientRect();
                cursor.children[0].children[1].style.width = `${rect.width}px`;
                cursor.children[0].children[1].style.height = `${rect.height}px`;
                cursor.children[0].children[1].style.borderRadius = "8px";
                cursor.children[0].children[1].style.border = "3px solid rgb(79 70 229 / 0.7)";
                cursor.children[0].children[0].style.background = "rgb(79 70 229)";
                coords1.set({ x: (rect.x + cursor.children[0].children[1].clientWidth/2)+3, y: (rect.y + cursor.children[0].children[1].clientHeight/2)+3 });
                coords2.set({ x: e.clientX, y: e.clientY });
            } else {
                cursor.children[0].children[1].style.width = `40px`;
                cursor.children[0].children[1].style.height = `40px`;
                cursor.children[0].children[1].style.border = "1px solid white";
                cursor.children[0].children[0].style.background = "white";
                cursor.children[0].children[1].style.borderRadius = "1000px";
                coords1.set({ x: e.clientX, y: e.clientY });
                coords2.set({ x: e.clientX, y: e.clientY });
            }
        }
        document.onmouseenter = () => cursor.style.opacity = `1`;
        document.onmouseleave = () => cursor.style.opacity = `0`;

        for(let i = 0;i< cardContainer.children.length;i++){
            setBallPos(i);
        }
    });

    const setBallPos = (i) => {
        let card = cardContainer.children[i];
        let ball = card.children[0];
        ball.style.opacity = "1";
        ball.style.top = `${Math.floor(Math.random() * (card.clientHeight - ball.clientHeight - 20)) + 10}px`;
        ball.style.left = `${Math.floor(Math.random() * (card.clientWidth - ball.clientWidth - 20)) + 10}px`;
        let duration = Math.floor(Math.random() * (4_000 - 1_000) + 1_000);
        ball.style.transitionDuration = `${duration}ms`;
        setTimeout(() => {setBallPos(i)}, duration);
    }

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

    let cursor;
    let cardContainer;

    const cards = [
        {
            title:"Angus",
            body:"My portfolio",
            preview:"//angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Portfolio",
            circleClasses:"bg-[rgb(70,148,83)]"
        },
        {
            title:"Shop",
            body:"My shop project",
            preview:"//shop.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Shop",
            circleClasses:"bg-[rgb(69,43,26)]"
        },
        {
            title:"FileHub",
            body:"Transfer files",
            preview:"//filehub.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/FileHub",
            circleClasses:"bg-[rgb(134,239,172)]"
        },
        {
            title:"Link Trim",
            body:"Shorten links",
            preview:"//linktrim.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/linkTrim",
            circleClasses:"bg-[rgb(25,154,70)]"
        },
        {
            title:"Sign It Now",
            body:"Sign petitions",
            preview:"//signitnow.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/SignItNow",
            circleClasses:"bg-[rgb(26,86,219)]"
        },
        {
            title:"Code Chronicles",
            body:"A blog where you can learn web development",
            preview:"//blog.angus.paillaugue.fr",
            code:"//github.com/Angus-Paillaugue/Code-Chronicles",
            circleClasses:"bg-[rgb(235,79,39)]"
        }
    ];
</script>

<svelte:head>
    <title>paillaugue.fr</title>
</svelte:head>

<div bind:this={cursor} class="w-full h-full z-50 transition-all fixed pointer-events-none">
    <div class="relative h-full w-full pointer-events-none">
        <!-- Point circle -->
        <div class="absolute -translate-x-1/2 -translate-y-1/2 rounded-full bg-white transition-colors" style="top: {$coords2.y}px; left: {$coords2.x}px; height: 5px; width: 5px;"></div>

        <!-- Outside circle -->
        <div class="absolute -translate-x-1/2 -translate-y-1/2 transition-colors" style="width: 30px; height: 30px; top: {$coords1.y}px; left: {$coords1.x}px; border-radius: 1000px; border: 1px solid white;"></div>
    </div>
</div>

<main class="min-h-screen flex flex-col w-full z-10">
    <div class="w-full max-w-screen-xl mx-auto px-4 md:px-6 py-24 grow items-center justify-center grid gap-6 lg:grid-cols-3 relative" bind:this={cardContainer}>
        {#each cards as card}
            <div class="relative group h-full w-full">
                <div class="absolute h-20 w-20 rounded-full {card.circleClasses}" style="opacity: 0; transition-property: all; transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1); transition-duration: 1s;"></div>
                <!-- <span class="w-1/2 aspect-square absolute blob {card.circleClasses} transition-all duration-1000"></span> -->
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