<script lang="ts">
  import type { SuperValidated } from '$lib';
  import SuperDebug from '$lib/client/SuperDebug.svelte';
  import type { PageData } from './$types';
  import TagForm from './TagForm.svelte';
  import type { schema } from './schema';

  export let data: PageData;

  let output: (string[] | undefined)[];
  let output2: (string[] | undefined)[];

  let validated: SuperValidated<typeof schema> | undefined;
  let validated2: SuperValidated<typeof schema> | undefined;
</script>

<h2>Nested forms</h2>

<h4>With direct client-side validation</h4>

<div class="forms">
  <TagForm bind:validated bind:output data={data.form} validator="zod" />
  <TagForm
    bind:validated={validated2}
    bind:output={output2}
    data={data.form2}
    validator="superforms"
  />
</div>

<pre style="margin-top:3rem;">
Zod validate:
{#if output}{output.join('\n')}{/if}
</pre>

<pre style="margin-top:3rem;">
Superforms validate:
{#if output2}{output2.join('\n')}{/if}
</pre>

<pre style="margin-top:3rem;">
Zod full validation:
{#if validated}{JSON.stringify(validated, null, 2)}{/if}
</pre>

<pre style="margin-top:3rem;">
Superforms full validation:
{#if validated2}{JSON.stringify(validated2, null, 2)}{/if}
</pre>

<style>
  .forms {
    display: flex;
    gap: 7rem;
  }
</style>
