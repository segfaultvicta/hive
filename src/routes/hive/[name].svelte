<script context="module">
  export async function preload(page, session) {
    const roomName = page.params.name;

    return { roomName: roomName };
  }
</script>

<script>
  import io from "socket.io-client";
  import { fade } from "svelte/transition";

  export let roomName;

  const socket = io();

  let messages = [];
  let message = "";
  let name = "Anonymous";

  socket.on("message", function (message) {
    messages = messages.concat(message);
  });

  socket.on("user joined", function ({ message }) {
    messages = messages.concat(message);
  });

  socket.on("user left", function () {});

  function emitUserDisconnect() {
    socket.emit("user disconnect", name);
  }

  function handleSubmit() {
    message = message.trim();

    if (message == "") {
      return;
    }

    let messageString = `${name}: ${message}`;

    messages = messages.concat(messageString);
    socket.emit("message", messageString);

    message = "";
  }
</script>

<style>

</style>

<svelte:head>
  <title>Hive - {roomName}</title>
</svelte:head>

<svelte:window on:unload={emitUserDisconnect} />
<body>
  <div class="main">
    <div id="chatWindow">
      <ul id="messages">
        {#each messages as message}
          <li transition:fade>{message}</li>
        {/each}
      </ul>
    </div>
    <form action="">
      <input id="m" autocomplete="off" bind:value={message} />
      <button on:click|preventDefault={handleSubmit}>Send</button>
    </form>
  </div>
</body>
