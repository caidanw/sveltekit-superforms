<script lang="ts">
  import type { PageData } from './$types';
  import { superForm } from '$lib/client';
  import { schema } from './schema';
  import SuperDebug from '$lib/client/SuperDebug.svelte';

  export let data: PageData;

  const { form, errors, message, enhance, tainted } = superForm(data.form, {
    validators: schema,
    validationMethod: 'oninput'
  });
</script>

<SuperDebug data={{ $form, $errors, $tainted }} />

<form method="POST" use:enhance>
  {#if $message}
    <div>Message: {$message}</div>
  {/if}

  <div>
    <label for="name1">Name 1</label>
    <select
      name="name1"
      data-invalid={$errors.name1}
      bind:value={$form.name1}
    >
      <option value="jack">Jack</option>
      <option value="jill">Jill</option>
    </select>
  </div>
  {#if $errors.name1}<div class="invalid">{$errors.name1}</div>{/if}

  <div>
    <label for="name2">Name 2</label>
    <select
      name="name2"
      data-invalid={$errors.name2}
      bind:value={$form.name2}
    >
      <option value="jack">Jack</option>
      <option value="jill">Jill</option>
    </select>
  </div>
  {#if $errors.name2}<div class="invalid">{$errors.name2}</div>{/if}

  <div><button>Submit</button></div>
</form>

<style>
  .invalid {
    color: red;
  }
</style>
