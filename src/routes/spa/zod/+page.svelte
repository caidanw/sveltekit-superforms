<script lang="ts">
  import { page } from '$app/stores';
  import { superForm, superValidateSync } from '$lib/client';
  import SuperDebug from '$lib/client/SuperDebug.svelte';
  import { schema } from './schema';

  const defaultData = {
    tags: [
      { id: 1, name: 'A' },
      { id: 2, name: 'Bb' },
      { id: 3, name: 'Cc' },
      { id: 4, name: 'Dd' }
    ],
    redirect: false,
    random: '4711'
  };

  console.log('Page loaded');

  const { form, errors, enhance, message } = superForm(
    superValidateSync(defaultData, schema),
    {
      SPA: true,
      dataType: 'json',
      onUpdate({ form, cancel }) {
        if ($page.url.searchParams.has('cancel')) cancel();
        else if (form.valid) {
          form.message = 'Successful!';
          form.data.random = String(Math.random()).slice(2);
        }
      },
      onUpdated({ form }) {
        console.log('onUpdated, valid:', form.valid);
      },
      validators: schema
    }
  );

  // <SuperDebug data={{ $form, $errors }} />
</script>

{#if $message}<h4>{$message}</h4>{/if}

<p>Random: {$form.random}</p>

<form method="POST" use:enhance>
  {#if $form.tags}
    {#each $form.tags as _, i}
      <div>
        <input
          type="number"
          data-invalid={$errors.tags?.[i]?.id}
          bind:value={$form.tags[i].id}
        />
        <input
          data-invalid={$errors.tags?.[i]?.name}
          bind:value={$form.tags[i].name}
        />
        {#if $errors.tags?.[i]?.id}
          <br />
          <span class="invalid">{$errors.tags[i].id}</span>
        {/if}
        {#if $errors.tags?.[i]?.name}
          <br />
          <span class="invalid">{$errors.tags[i].name}</span>
        {/if}
      </div>
    {/each}
  {/if}
  <button>Submit</button>
</form>

<style lang="scss">
  button {
    margin-right: 20px;
  }

  input:not([type='checkbox']) {
    width: 100px;
  }

  .invalid {
    color: red;
  }
</style>
