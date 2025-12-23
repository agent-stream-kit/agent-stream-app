<script lang="ts">
  import type { AgentDefinitions } from "tauri-plugin-askit-api";

  import * as Collapsible from "$lib/components/ui/collapsible/index.js";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

  import AgentListItems from "./agent-list-item.svelte";

  interface Props {
    categories: Record<string, any>;
    agentDefs: AgentDefinitions;
    expandAll?: boolean;
    onDragAgentStart?: (event: DragEvent, agentName: string) => void;
  }

  let { categories, agentDefs, expandAll = false, onDragAgentStart }: Props = $props();

  const categoryKeys = $derived(Object.keys(categories).sort());
</script>

{#each categoryKeys as key}
  {#if key === "00agents"}
    {@const agentNames = categories[key].sort()}
    {#each agentNames as agentName}
      <Sidebar.MenuSubItem
        class="font-light"
        draggable={true}
        ondragstart={(event) => onDragAgentStart?.(event, agentName)}
      >
        {agentDefs[agentName].title ?? agentName}
      </Sidebar.MenuSubItem>
    {/each}
  {:else}
    <Collapsible.Root open={expandAll} class="group/collapsible">
      <Sidebar.MenuItem>
        <Collapsible.Trigger>
          {#snippet child({ props })}
            <Sidebar.MenuButton class="text-base" {...props}>
              {key}
            </Sidebar.MenuButton>
          {/snippet}
        </Collapsible.Trigger>
        <Collapsible.Content>
          <Sidebar.MenuSub>
            <AgentListItems
              categories={categories[key]}
              {agentDefs}
              {expandAll}
              {onDragAgentStart}
            />
          </Sidebar.MenuSub>
        </Collapsible.Content>
      </Sidebar.MenuItem>
    </Collapsible.Root>
  {/if}
{/each}
