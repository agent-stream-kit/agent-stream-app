<script lang="ts">
  import EllipsisIcon from "@lucide/svelte/icons/ellipsis";

  import { Button } from "$lib/components/ui/button/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

  let { data } = $props();

  let open = $state(false);
</script>

<div class="flex items-center gap-2 text-sm">
  <Popover.Root bind:open>
    <Popover.Trigger>
      {#snippet child({ props })}
        <Button {...props} variant="ghost" size="icon" class="data-[state=open]:bg-accent size-7">
          <EllipsisIcon />
        </Button>
      {/snippet}
    </Popover.Trigger>
    <Popover.Content class="w-56 overflow-hidden rounded-lg p-0" align="end">
      <Sidebar.Root collapsible="none" class="bg-transparent">
        <Sidebar.Content>
          {#each data as group, index (index)}
            <Sidebar.Group class="border-b last:border-none">
              <Sidebar.GroupContent class="gap-0">
                <Sidebar.Menu>
                  {#each group as item, index (index)}
                    <Sidebar.MenuItem>
                      <Sidebar.MenuButton
                        class="hover:bg-accent hover:text-accent-foreground"
                        onclick={() => {
                          item.onclick?.();
                          open = false;
                        }}
                      >
                        <item.icon /> <span>{item.label}</span>
                      </Sidebar.MenuButton>
                    </Sidebar.MenuItem>
                  {/each}
                </Sidebar.Menu>
              </Sidebar.GroupContent>
            </Sidebar.Group>
          {/each}
        </Sidebar.Content>
      </Sidebar.Root>
    </Popover.Content>
  </Popover.Root>
</div>
