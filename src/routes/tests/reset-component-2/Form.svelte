<script lang="ts">
  import SuperDebug from '$lib/client/SuperDebug.svelte';

  import { superForm } from '$lib/client';
  export let data;
  export let schema: any | undefined = undefined;
  export let dataType: 'form' | 'json' = 'form';
  export let invalidateAll; // set to false to keep form data using muliple forms on a page

  export const _form = superForm(data, {
    dataType: dataType,
    validators: schema ? schema : undefined,
    invalidateAll: invalidateAll,
    onError({ result }) {
      $message = {
        text: result?.error?.message,
        status: 500
      };
    },
    onUpdated({ form }) {
      if (form.valid) {
        // Successful post! Do some more client-side stuff.
      }
    }
  });

  const { form, message, tainted, delayed, errors, allErrors, enhance } =
    _form;
</script>

<form method="POST" use:enhance {...$$restProps}>
  <slot
    form={_form}
    message={$message}
    errors={$errors}
    allErrors={$allErrors}
    delayed={$delayed}
  />
</form>

<SuperDebug data={$form} />
