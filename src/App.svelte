<script lang="ts">
	import Emoji from "./Emoji"

	let text=''
	let index=-1
	let inputElement:HTMLInputElement
	let lastUsed:string[]=JSON.parse(localStorage.getItem('lastUsedEmojis') ?? '[]') ?? []

	$: console.log({lastUsed})

	$: {
		localStorage.setItem('lastUsedEmojis',JSON.stringify(lastUsed))
	}

	$: {
		console.log({index})

		if (index === -1) {
			inputElement?.setSelectionRange(
				inputElement.value.length,
				inputElement.value.length
			)
		}else{
			inputElement?.setSelectionRange(index, index)
		}
	}

  	const OnChange=(event:Event & { currentTarget: EventTarget & HTMLInputElement })=>{
		text = event.currentTarget?.value.replaceAll(
			/(:[^:]+:)/gi,
			name => {
				index = event.currentTarget?.value.indexOf(name)+1
				console.log({val:event.currentTarget?.value,name,index})

				const emojiName = name.slice(1, -1).toLowerCase()

				const findResult = Emoji.find(
					({ name }) => name === emojiName
				)?.emoji
				

			if (findResult){
				if (lastUsed.length===10)lastUsed.shift()	
				lastUsed=[...lastUsed, findResult]
			}

			return findResult ?? name
			}
		)
  	}
</script>

<main>
  	<input type="text" value={text} on:input={OnChange} bind:this={inputElement}>

  	<div>
		{#each lastUsed as lastUsedIter}
			<p>{lastUsedIter}</p>
		{/each}
	</div>
</main>

<style>
</style>
