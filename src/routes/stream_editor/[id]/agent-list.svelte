<script lang="ts">
  import { type ComponentProps } from "svelte";

  import type { AgentDefinitions } from "tauri-plugin-askit-api";

  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

  import AgentListItems from "./agent-list-item.svelte";

  type Props = ComponentProps<typeof Sidebar.Root> & {
    agentDefs: AgentDefinitions;
    onAddAgent: (agentName: string, position?: { x: number; y: number }) => Promise<void>;
    onDragAgentStart?: (event: DragEvent, agentName: string) => void;
  };

  let { agentDefs, onAddAgent, onDragAgentStart, ...restProps }: Props = $props();

  let searchTerm = $state("");

  const filteredAgentDefs = $derived(
    Object.fromEntries(
      Object.entries(agentDefs).filter(([_name, def]) => {
        const term = searchTerm.trim().toLowerCase();
        if (!term) {
          return true;
        }
        const title = (def.title ?? "").toLowerCase();
        return title.includes(term);
      }),
    ),
  );

  const EXPAND_THRESHOLD = 8;
  const expandAll = $derived(Object.keys(filteredAgentDefs).length <= EXPAND_THRESHOLD);

  const categories = $derived(
    Object.keys(filteredAgentDefs).reduce(
      (acc, key) => {
        const categoryPath = (filteredAgentDefs[key].category ?? "_unknown_").split("/");
        let currentLevel = acc;

        for (const part of categoryPath) {
          if (!currentLevel[part]) {
            currentLevel[part] = {};
          }
          currentLevel = currentLevel[part];
        }

        if (!currentLevel["00agents"]) {
          currentLevel["00agents"] = [];
        }
        currentLevel["00agents"].push(key);

        return acc;
      },
      {} as Record<string, any>,
    ),
  );
</script>

<Sidebar.Root collapsible="none" variant="floating" class="rounded-md" {...restProps}>
  <Sidebar.Header>
    <div class="mb-2 flex items-center justify-between gap-2">
      <div class="text-primary font-bold mb-0">Agents</div>
      <input
        type="search"
        class="w-38 ml-2 mr-10 rounded-md border border-gray-800 bg-gray-900 px-1 py-0.5 text-sm text-gray-100 focus:border-gray-400 focus:outline-none focus:ring-1 focus:ring-gray-400"
        bind:value={searchTerm}
      />
    </div>
  </Sidebar.Header>
  <Sidebar.Content>
    <Sidebar.Menu>
      <AgentListItems {categories} {agentDefs} {expandAll} {onDragAgentStart} />
    </Sidebar.Menu>
  </Sidebar.Content>
</Sidebar.Root>
