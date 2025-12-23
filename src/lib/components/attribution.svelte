<script>
  import { getVersion } from "@tauri-apps/api/app";
  import { open } from "@tauri-apps/plugin-shell";

  function onclick() {
    open("https://github.com/agent-stream-kit/agent-stream-app");
  }
</script>

<div class="text-xs">
  <button {onclick}>
    {#await getVersion() then version}
      {version}
    {/await}
  </button>
</div>
