<script>
  import { supabase } from '$lib/supabaseClient'
  import { goto } from '$app/navigation'

  let title = ''
  let description = ''

  async function addDeck() {
    const { error } = await supabase.from('decks').insert([{ title, description }])
    if (error) {
      alert(error.message)
    } else {
      goto('/decks')
    }
  }
</script>

<form on:submit|preventDefault={addDeck}>
  <input type="text" bind:value={title} placeholder="Title" />
  <textarea bind:value={description} placeholder="Description"></textarea>
  <button type="submit">Add Deck</button>
</form>
