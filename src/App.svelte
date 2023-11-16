<script lang="ts">
	import Emoji from "./Emoji"

  let text=''
  let index=-1

  $: console.log({text})
  $: console.log({index})

  const OnChange=(event:Event & { currentTarget: EventTarget & HTMLInputElement })=>{
    text = event.currentTarget?.value.replaceAll(
			/(:[^:]+:)/gi,
			name => {
        console.log({name})
				index = event.currentTarget?.value.indexOf(name)

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
  <input type="text" value={text} on:input={OnChange}>
</main>

<style>
</style>
