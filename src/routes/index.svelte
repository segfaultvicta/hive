<script>
  import io from "socket.io-client";

  const socket = io();
</script>

<style>
  .main {
    margin-top: 0px;
  }
</style>

<svelte:head>
  <title>Hive</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</svelte:head>

<body>
  <div class="main">
    This is just placeholder bullshit. Go to /hive/[name] to create a new hive,
    or join one that already exists.
  </div>
</body>
