<script>
	import { onMount } from 'svelte'
	import { goto } from '$app/navigation'
	import { supabase } from '$lib/supabaseClient'
	import { page } from '$app/stores'
	import { get } from 'svelte/store'

	let title = ''
	let description = ''
	let deckId

	// Extract the deck ID from the page URL
	$: deckId = get(page).params.id

	// Fetch the deck's current details
	onMount(async () => {
		const { data, error } = await supabase.from('decks').select('*').eq('id', deckId).single()

		if (error) {
			console.error(error.message)
		} else {
			title = data.title
			description = data.description
		}
	})

	// Function to update the deck's details
	async function updateDeck() {
		const { error } = await supabase.from('decks').update({ title, description }).eq('id', deckId)

		if (error) {
			alert(error.message)
		} else {
			goto('/decks')
		}
	}
</script>

<form on:submit|preventDefault={updateDeck}>
	<label for="title">Title</label>
	<input id="title" type="text" bind:value={title} />

	<label for="description">Description</label>
	<textarea id="description" bind:value={description}></textarea>

	<button type="submit">Update Deck</button>
</form>

<button on:click={() => goto('/decks')}>Back to Decks</button>
