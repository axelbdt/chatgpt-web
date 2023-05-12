<script lang="ts">
  import { createEventDispatcher } from "svelte";

  export let active = false;
  export let initialChatName = "";

  const dispatch = createEventDispatcher();
  let localChatName = "";
  let chatNameInput: HTMLInputElement;

  const focusInput = () => {
    if (chatNameInput && active) {
      chatNameInput.focus();
    }
  };

  $: {
    if (active) {
      localChatName = initialChatName;
      focusInput();
    }
  }

  const close = () => {
    active = false;
  };

  const save = () => {
    dispatch("save", localChatName);
    active = false;
  };
</script>

<div class={active ? "modal is-active" : "modal"}>
  <div class="modal-background" on:click={close} />
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Enter a new name for this chat</p>
    </header>
    <section class="modal-card-body">
      <div class="field is-horizontal">
        <div class="field-label is-normal">
          <label class="label" for="settings-chat-name">New name:</label>
        </div>
        <div class="field-body">
          <div class="field">
            <input
              class="input"
              type="text"
              id="settings-chat-name"
              bind:value={localChatName}
              bind:this={chatNameInput}
            />
          </div>
        </div>
      </div>
    </section>
    <footer class="modal-card-foot">
      <button class="button is-info" on:click={save}>Save</button>
      <button class="button" on:click={close}>Cancel</button>
    </footer>
  </div>
</div>
