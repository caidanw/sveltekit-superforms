<script lang="ts">
  import { superForm } from '$lib/client';
  import type { PageData } from './$types';
  import { schema1 } from './schema';

  export let data: PageData;

  const { form, enhance } = superForm(data.form1, {
    validators: schema1
  });
</script>

<form method="POST" use:enhance>
  <input type="text" bind:value={$form.age} name="age" />
  <input type="text" bind:value={$form.value} name="value" />
</form>
