<script>
  import { v4 } from "uuid";
  import Alert from "./assets/alerts.svelte";

  let notes = JSON.parse(localStorage.getItem('notes')) || [];
  
  let status = "";
  let msg = "";

  let note_schema = { id: "", title: "", description: "" };
  let edit = false;
  function cleanform() {
    note_schema = { id: "", title: "", description: "" };
  }
  function addnote() {
    if (note_schema.title && note_schema.description) {
      const note = {
        id: v4(),
        title: note_schema.title,
        description: note_schema.description,
      };
      notes = notes.concat(note);
      localStorage.setItem('notes',JSON.stringify(notes))
      status = 'info'
      msg = "The note was saved successfully";
    } else {
      status = 'warning'
      msg =
        "Title and Description cannot be empty. Remember to fill all the fields";
    }
  }

  function updatenote() {
    const note = {
      id: note_schema.id,
      title: note_schema.title,
      description: note_schema.description,
    };

    const index = notes.findIndex((i) => i.id === note.id);
    notes[index] = note;
    localStorage.setItem('notes',JSON.stringify(notes))
    edit = false;
    msg = 'Note was updated successfully'
    status = 'info'
  }
  function onsubmit(e) {
    if (!edit) {
      addnote();
    } else {
      updatenote();
    }
    setTimeout(() => (status=''), 4000);
    cleanform();
  }
  function ondelete(id) {
    notes = notes.filter((note) => note.id !== id);
    localStorage.setItem('notes',JSON.stringify(notes))
    msg = 'The note was removed successfully'
    status = 'deletion'
    setTimeout(() => (status=''), 4000);
  }
  function onedit(EditedNote) {
    note_schema = EditedNote;
    edit = true;
  }
</script>

<main>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form action="" on:submit|preventDefault={onsubmit}>
              <h1>Notes app</h1>
              <div class="form-item">
                <input
                  type="text"
                  bind:value={note_schema.title}
                  class="form-control"
                  placeholder="Title"
                />
              </div>

              <div class="form-item">
                <textarea
                  type="text"
                  placeholder="Description"
                  rows="5"
                  class="form-control description"
                  bind:value={note_schema.description}
                />
              </div>

              <button type="submit" class="btn btn-primary">
                {#if edit}Edit Note
                {:else}Save Note{/if}
              </button>
            </form>
          </div>
        </div>
        <Alert msg={msg} status={status} />
      </div>
      <div class="col-md-6">
        {#each notes as note}
          <div class="card">
            <div class="row">
              <div class="col-md-7">
                <div class="card-body">
                  <strong><h3>{note.title}</h3></strong>
                  <p class="text">{note.description}</p>
                  <button class="btn btn-secondary" on:click={onedit(note)}
                    >Edit</button
                  >
                  <button on:click={ondelete(note.id)} class="btn btn-danger"
                    >Delete</button
                  >
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</main>
