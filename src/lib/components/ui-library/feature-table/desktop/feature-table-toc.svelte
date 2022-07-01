<script lang="ts">
  import FeatureTableHeader from "./feature-table-header.svelte";
  import FeatureTableTocItem from "./feature-table-toc-item.svelte";
  import type { FeatureTableToc } from "../feature-table.types";
  export let tocData: FeatureTableToc[];
</script>

<div id="toc" class="p-4">
  <FeatureTableHeader isToc={true} />
  <section class="grid grid-cols-1 auto-rows-[3.75rem] pt-4 inner-grid-desktop">
    {#each tocData as item}
      {#if item.isHeadline}
        <div class="-ml-8 font-semibold">{item.headlineText}</div>
      {:else}
        <FeatureTableTocItem type={item.type} definition={item.data} />
      {/if}
    {/each}
  </section>
</div>
