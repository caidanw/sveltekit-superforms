<script lang="ts">
  import type { PageData } from './$types';

  import { page } from '$app/stores';
  import { superForm } from '$lib/client';
  import { schema } from './schema';

  export let data: PageData;

  const validationMethod =
    ($page.url.searchParams.get('method') as any) ?? 'auto';

  const { form, errors, message, enhance } = superForm(data.form, {
    customValidity: true,
    validators: schema,
    validationMethod,
    taintedMessage:
      validationMethod == 'auto'
        ? 'You have unsaved changes. Leave page?'
        : null
  });
</script>

{#if $message}<h4>{$message}</h4>{/if}

<form novalidate method="POST" use:enhance>
  <label>
    Name: <input name="name" bind:value={$form.name} />
  </label>

  <label>
    Email: <input type="email" name="email" bind:value={$form.email} />
  </label>

  <label>
    Number: <input type="number" name="number" bind:value={$form.number} />
  </label>

  <label>
    Info: <input
      type="TEXt"
      name="info"
      data-no-custom-validity
      bind:value={$form.info}
    />
    {#if $errors.info}<span class="invalid">{$errors.info}</span>{/if}
  </label>

  <label>
    <select name="menu" bind:value={$form.menu}>
      <option value="">Select an option</option>
      <option value="first">First</option>
      <option value="second">Second</option>
      <option value="third">Third</option>
    </select>
    {#if $errors.menu}<span class="invalid">{$errors.menu}</span>{/if}
  </label>

  <label>
    Radio:<br />
    <input name="radio" type="radio" bind:group={$form.radio} value={0} />
    0<br />
    <input name="radio" type="radio" bind:group={$form.radio} value={1} />
    1<br />
    <input name="radio" type="radio" bind:group={$form.radio} value={2} />
    2

    <label>
      Text: <textarea name="text" bind:value={$form.text} />
    </label>

    <label>
      Accept terms: <input
        type="checkbox"
        name="accept"
        bind:checked={$form.accept}
      />
    </label>
    <div>
      <button>Submit</button>
    </div>
  </label>
</form>

{#if validationMethod != 'auto'}
  <p>Validation method: <b>{validationMethod}</b></p>
{/if}

<!--SuperDebug data={{ $form, $errors }} /-->

<style lang="scss">
  form {
    margin: 2rem 0;

    input {
      background-color: #dedede;
    }

    .invalid {
      color: crimson;
    }
  }
</style>
