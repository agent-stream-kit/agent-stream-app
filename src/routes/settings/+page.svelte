<script lang="ts">
  import { Separator } from "$lib/components/ui/separator/index.js";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

  import Agent from "./Agent.svelte";
  import Core from "./Core.svelte";

  const { data } = $props();

  const settings = $derived(data.coreSettings);
  const agentGlobalConfigsMap = $derived(data.agentGlobalConfigsMap);
  const agentDefs = $derived(data.agentDefs);
</script>

<main class="container mx-auto p-8 space-y-8 mt-20">
  <header class="flex-none h-14 items-centger">
    <Sidebar.Trigger />
    <Separator orientation="vertical" class="me-2 data-[orientation=vertical]:h-4" />
  </header>
  <h1 class="text-xl font-semibold sm:text-2xl">Settings</h1>
  <Core {settings} />

  <h2 class="text-xl font-semibold sm:text-2xl">Agents</h2>
  {#each Object.entries(agentGlobalConfigsMap) as [agentName, agentConfigs]}
    <Agent {agentName} {agentConfigs} agentDef={agentDefs[agentName]} />
  {/each}
</main>
