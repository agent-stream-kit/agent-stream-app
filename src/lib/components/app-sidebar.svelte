<script lang="ts">
  import type { ComponentProps } from "svelte";
  import { onMount } from "svelte";

  import SettingsIcon from "@lucide/svelte/icons/settings";
  import WorkflowIcon from "@lucide/svelte/icons/workflow";

  import { useSidebar } from "$lib/components/ui/sidebar/context.svelte.js";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

  import Attribution from "./attribution.svelte";
  import NavMain from "./nav-main.svelte";
  import NavSecondary from "./nav-secondary.svelte";

  const data = {
    navMain: [
      {
        title: "Streams",
        url: "/",
        icon: WorkflowIcon,
      },
    ],
    navSecondary: [
      {
        title: "Settings",
        url: "/settings",
        icon: SettingsIcon,
      },
    ],
  };

  let { ...restProps }: ComponentProps<typeof Sidebar.Root> = $props();

  let sidebar = useSidebar();

  onMount(() => {
    sidebar.setOpen(false);
  });
</script>

<Sidebar.Root collapsible="icon" {...restProps}>
  <Sidebar.Header>
    <Sidebar.MenuItem>
      <Sidebar.MenuButton
        size="lg"
        class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
      >
        <div
          class="bg-sidebar-primary text-sidebar-primary-foreground flex aspect-square size-8 items-center justify-center rounded-lg"
        >
          <WorkflowIcon class="size-4" />
        </div>
        <div class="grid flex-1 text-start text-sm leading-tight">
          <span class="truncate font-medium"> Agent Stream App </span>
        </div>
      </Sidebar.MenuButton>
    </Sidebar.MenuItem>
  </Sidebar.Header>
  <Sidebar.Content>
    <NavMain items={data.navMain} class="mt-auto" />
    <NavSecondary items={data.navSecondary} class="mt-auto" />
  </Sidebar.Content>
  <Sidebar.Footer>
    <div class="px-1">
      <Attribution />
    </div>
  </Sidebar.Footer>
</Sidebar.Root>
