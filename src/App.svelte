<script>
  import Router from "svelte-spa-router";
  import PlanetList from "./PlanetList.svelte";
  import PlanetDetail from "./PlanetDetail.svelte";

  const routes = {
    "/": PlanetList,
    "/planet/:id": PlanetDetail,
  };
</script>

<Router {routes} />
