<script>
  import { Router, Route, Link } from "svelte-routing";
  import Home from "./Home.svelte";
  import GithubUsers from "./GithubUsers.svelte";

  // components
  import Navbar from "./Navbar.svelte";

  export let url = "";
</script>

<Router {url}>
  <div>
    <Route path="/">
      <Home />
    </Route>
    <Route path="/githubUsers">
      <GithubUsers />
    </Route>
  </div>

</Router>
