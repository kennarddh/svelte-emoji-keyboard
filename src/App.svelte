<script lang="ts">
	import Emoji from "./Emoji"

  let text=''
  let index=-1
  let inputElement:HTMLInputElement

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

				return findResult ?? name
			}
		)
  }
</script>

<main>
  <input type="text" value={text} on:input={OnChange} bind:this={inputElement}>
</main>

<style>
</style>
