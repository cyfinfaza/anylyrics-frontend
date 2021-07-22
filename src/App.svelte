<script>
	import typewriter from './typewriter'
	// let lines = ["I've been tripping, no one's perfect", "Chasing vision, just the surface", "Shirt\u2019s on backwards, not on purpose, yeah", "I've been learning, something bigger", "Expectations, feet were failing", "I found blessings flowing from the sight of heaven", "\u00a0", "Staring into my reflection, redirecting my perfection somewhere else", "When there isn't any progress, lean on truth inside the promise, it is well", "\u00a0", "I hold my life out in front of me, dreams of who I want to be", "I'm seeing every empty page", "But I find that everything I am is everything I should be", "Yeah, I don't need to run away", "(Ooh-oh-ooh)", "I don't need to run away", "(Ooh-oh-ooh)", "I don't need to run away", "\u00a0", "Something's working, heart is turning", "Vision\u2019s clear and still I\u2019m learning", "That what I am, what I am, what I am, what I am", "Is something more than I can plan", "Go, tell me now (ooh-ooh-ooh-ooh)", "I don't need to run away", "\u00a0", "I've been standing on a stage or just a mirror, I\u2019m forced to face who I become (ooh-ooh-ooh)", "Searching for a new escape, I scan the exits that embrace an easy out (ooh-ooh-ooh)", "\u00a0", "I hold my life out in front of me, dreams of who I want to be", "I'm seeing every empty page", "But I find that everything I am is everything I should be", "Yeah, I don't need to run away", "(Ooh-oh-ooh)", "I don't need to run away", "(Ooh-oh-ooh)", "(I don\u2019t need to run away)", "I don't need to run away", "(Ooh-oh-ooh)", "I don't need to run away, yeah", "\u00a0", "I don't need to run away", "I don't need to run away"];
	let lines = []
	let searching = false;
	let query = "";
	let error = false;
	async function doSearch(){
		if(query.length > 0){
			searching = true
			let data = await fetch(`https://us-south.functions.appdomain.cloud/api/v1/web/cyfinfaza%40gmail.com_dev/default/ai-lyrics?q=${query}`).then(response => response.json())
			// let data = await fetch(`http://localhost:5006/lyrics?q=${query}`).then(response => response.json())
			if(data.error || !data){
				error = true
			}
			else{
				lines = data.data
			}
			searching = false
		}
	}
	// function typelines(node){
	// 	console.log(node.children)
	// 	const all = node.children
	// 	node.children = []
	// 	return{
	// 		duration:3000,
	// 		tick: t=>{
	// 			const i = ~~(t * all.length)
	// 			for(n=0; n<i; n++){
	// 				node.appendChild(all[n])
	// 			}
	// 		}
	// 	}
	// }
</script>

<main>
	<div style="text-align: right;">
		<!-- <p style="margin-bottom: 0;">Welcome to</p> -->
		<h1 style="margin-bottom: 0;">Cy2 AnyLyrics</h1>
		<p style="margin-top: 0;">beta</p>
		<p>An AI-powered web scraper for finding song lyrics.</p>
		<p style="font-size: 0.7em;">AnyLyrics's model is not completely trained, so many results may be only partially complete. Use at your own risk. You can find the API by reverse-engineering this page.</p>
		<p>
			<!-- <button on:click={_=>searching=!searching}>toggle searching</button> -->
		</p>
	</div>
	<div class="rightColumn">
		<div style="line-height: 2em;">
			<input type="text" placeholder="query a song" bind:value={query} on:keydown={e=>{if(e.key=="Enter") doSearch()}} autofocus />
			<button on:click={_=>doSearch()}>find lyrics</button>
		</div>
		{#if searching}
			<p class="loading">searching...
				<br>this can take up to 20 seconds
			</p>
		{:else}
			{#if error}
				<p style="color: red;" >An error has occurred</p>
			{:else if lines.length>0}
				<p class="lyricsContainer">
					{#each lines as line}
						<span>{line}</span><br>
					{/each}
				</p>
			{:else}
				<p style="opacity: 0.5;" >lyrics will appear here when found</p>
			{/if}
		{/if}	
	</div>
</main>

<style>
	main {
		display: flex;
		align-items: center;
		gap: 2em;
		max-height: 100%;
		padding: 32px;
		height: 100%;
		box-sizing: border-box;
	}

	@media screen and (max-width: 875px) {
		main {
			flex-direction: column;
			height: auto;
			max-height: 100%;
		}
	}

	main>*{
		max-width: 600px;
		max-height: 100%;
		width: 100%;
	}

	.rightColumn {
		display: flex;
		flex-direction: column;
		max-height: 600px;
		position: relative;
		/* max-width: 600px; */
	}

	.rightColumn>*:last-child{
		margin-bottom:0;
	}

	.lyricsContainer {
		overflow: auto;
		/* padding-top: 16px; */
		padding-bottom: 16px;
	}

	.lyricsContainer::-webkit-scrollbar{
		width: 8px;
	}

	.lyricsContainer::-webkit-scrollbar-track{
		background-color: transparent;
	}

	.lyricsContainer::-webkit-scrollbar-thumb{
		background-color: #fff2;
	}

	.lyricsContainer::-webkit-scrollbar-thumb:hover{
		background-color: #fff4;
	}

	.lyricsContainer::-webkit-scrollbar-thumb:active{
		background-color: #fff3;
	}

	.lyricsContainer::before, .lyricsContainer::after {
		position: absolute;
		/* top: 0; */
		content: "";
		width: 100%;
		height: 10px;
		left: 0;
	}

	.lyricsContainer::before {
		background: linear-gradient(to bottom, black, transparent);
		/* top: 0;
		left: 0;
		right: 0;
		position: sticky; */
	}
	.lyricsContainer::after {
		background: linear-gradient(to top, black, transparent);
		bottom: 0;
	}


	/* main>*>*{
		margin-bottom: 0;
	} */

	.loading {
		background: linear-gradient(-60deg, #fff2, white, #fff2);
		animation: loading-background 2s infinite linear;
		background-size: 200%;
		-webkit-background-clip: text;
		background-clip: text;
		color: transparent;
	}

	@keyframes loading-background {
		from {
			background-position: 200%;
		}
		to {
			background-position: 0%;
		}
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>