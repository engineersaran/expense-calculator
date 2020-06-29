<script>
  import { Link } from "svelte-routing";
  export let showForm;
  export let isHome = true;
</script>

<nav class="nav">
  <div class="nav-center">
    {#if isHome}
      <Link to="/">
        <h1 class="nav-title">Expense Calculator</h1>
      </Link>
      <button class="nav-btn" on:click={showForm}>
        <i class="far fa-plus-square" />
        Add Expense
      </button>
      <Link to="/githubUsers">
        <button class="nav-btn">
          <i class="far fa-plus-square" />
          GitHub users
        </button>
      </Link>
    {:else}
      <Link to="/">
        <h1 class="nav-title">Take me Home</h1>
      </Link>
    {/if}
  </div>

</nav>
