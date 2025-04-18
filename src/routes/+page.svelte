<script>
  let notes = [];
  let selectedId = null;

  function addNote() {
    const newNote = { id: Date.now(), title: '', content: '' };
    notes = [newNote, ...notes];
    selectedId = newNote.id;
  }

  function deleteNote(id) {
    notes = notes.filter((n) => n.id !== id);
    if (selectedId === id) {
      selectedId = notes.length ? notes[0].id : null;
    }
  }

  $: selectedNote = notes.find((n) => n.id === selectedId);
</script>

<div class="flex h-screen bg-gray-100">
  <aside class="w-1/4 bg-white border-r overflow-y-auto p-4">
    <button on:click={addNote} class="w-full bg-blue-500 hover:bg-blue-600 text-white py-2 px-4 rounded mb-4">
      + New Note
    </button>
    <ul class="divide-y">
      {#each notes as note}
        <li
          class="py-2 px-2 flex justify-between items-center hover:bg-gray-100 cursor-pointer {note.id === selectedId ? 'bg-gray-200' : ''}"
          on:click={() => (selectedId = note.id)}
        >
          <span class="truncate">{note.title || 'Untitled'}</span>
          <button
            on:click|stopPropagation={() => deleteNote(note.id)}
            class="text-red-500 hover:text-red-700"
          >
            &times;
          </button>
        </li>
      {/each}
    </ul>
  </aside>
  <main class="flex-1 p-4">
    {#if selectedNote}
      <input
        bind:value={selectedNote.title}
        placeholder="Title"
        class="w-full text-2xl font-semibold border-b mb-4 outline-none"
      />
      <textarea
        bind:value={selectedNote.content}
        placeholder="Content"
        class="w-full h-full resize-none p-2 border rounded focus:outline-none"
      ></textarea>
    {:else}
      <p class="text-gray-500">Create or select a note to begin.</p>
    {/if}
  </main>
</div>
