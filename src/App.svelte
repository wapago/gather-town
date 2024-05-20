<script>
  import Router from "svelte-spa-router";
  import Main from "./pages/Main.svelte";

  const routes = {
    "/": Main,
  };
</script>

<Router {routes} />
