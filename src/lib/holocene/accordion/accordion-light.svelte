<script lang="ts">
  import type { HTMLAttributes } from 'svelte/elements';
  import { noop } from 'svelte/internal';
  import { slide } from 'svelte/transition';

  import { v4 } from 'uuid';

  import type { IconName } from '$lib/holocene/icon';
  import Icon from '$lib/holocene/icon/icon.svelte';

  interface $$Props extends HTMLAttributes<HTMLDivElement> {
    id?: string;
    icon?: IconName;
    open?: boolean;
    expandable?: boolean;
    error?: string;
    onToggle?: () => void;
    'data-testid'?: string;
  }

  export let id: string = v4();
  export let open = false;
  export let onToggle = noop;

  const toggleAccordion = () => {
    open = !open;
    onToggle();
  };
</script>

<div class="w-full" {...$$restProps}>
  <button
    id="{id}-trigger"
    aria-expanded={open}
    aria-controls="{id}-content"
    class="focus-visible:outline-interactive w-full cursor-pointer hover:bg-interactive-secondary-hover"
    type="button"
    on:click={toggleAccordion}
  >
    <div class="flex w-full flex-row items-center justify-between gap-2">
      <slot name="title" />
      <slot name="description" />
      <div
        class="flex flex-row items-center gap-2 pr-2"
        on:click|stopPropagation
        on:keyup|stopPropagation
        role="none"
      >
        <slot name="action" />
        <Icon class="m-2 shrink-0" name={open ? 'arrow-down' : 'arrow-right'} />
      </div>
    </div>
  </button>
  <div
    id="{id}-content"
    aria-labelledby="{id}-trigger"
    role="textbox"
    class="block w-full bg-primary p-2"
    class:hidden={!open}
    transition:slide
  >
    <slot {open} />
  </div>
</div>
