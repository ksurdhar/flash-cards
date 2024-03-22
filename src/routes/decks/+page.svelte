<script>
  import { onMount } from 'svelte'
  import { supabase } from '$lib/supabaseClient'

  let decks = []

  async function fetchDecks() {
    const { data, error } = await supabase.from('decks').select('*')
    if (error) {
      console.error(error)
    } else {
      decks = data
    }
  }

  async function deleteDeck(id) {
    await supabase.from('decks').delete().match({ id })
    fetchDecks() // Refresh the list after deletion
  }

  onMount(fetchDecks)
</script>

{#each decks as { id, title, description }}
  <div>
    <h2>{title}</h2>
    <p>{description}</p>
    <button on:click={() => deleteDeck(id)}>Delete</button>
  </div>
{/each}
