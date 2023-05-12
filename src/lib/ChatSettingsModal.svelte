<script lang="ts">
  import { createEventDispatcher } from "svelte";

  export let settingsMap;
  export let active = false;

  const dispatch = createEventDispatcher();
  let settings: HTMLDivElement;

  const close = () => {
    dispatch("close");
  };
  const clear = () => {
    dispatch("clear");
  };
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class={active ? "modal is-active" : "modal"}>
  <div class="modal-background" on:click={close} />
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Settings</p>
    </header>
    <section class="modal-card-body">
      <p class="notification is-warning">
        Below are the settings that OpenAI allows to be changed for the API
        calls. See the <a
          href="https://platform.openai.com/docs/api-reference/chat/create"
          >OpenAI API docs</a
        > for more details.
      </p>
      {#each settingsMap as setting}
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label" for="settings-{setting.key}"
              >{setting.name}</label
            >
          </div>
          <div class="field-body">
            <div class="field">
              {#if setting.type === "number"}
                <input
                  class="input"
                  inputmode="decimal"
                  type={setting.type}
                  title={setting.title}
                  id="settings-{setting.key}"
                  min={setting.min}
                  max={setting.max}
                  step={setting.step}
                  placeholder={String(setting.default)}
                />
              {:else if setting.type === "select"}
                <div class="select">
                  <select id="settings-{setting.key}" title={setting.title}>
                    {#each setting.options as option}
                      <option
                        value={option}
                        selected={option === setting.default}>{option}</option
                      >
                    {/each}
                  </select>
                </div>
              {/if}
            </div>
          </div>
        </div>
      {/each}
    </section>

    <footer class="modal-card-foot">
      <button class="button is-info" on:click={close}>Close settings</button>
      <button class="button" on:click={clear}>Clear settings</button>
    </footer>
  </div>
</div>
