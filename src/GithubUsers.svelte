<script>
  import Navbar from "./Navbar.svelte";
  async function getGithubUsers() {
    let userData = await fetch("https://api.github.com/users");
    let githubUsers = await userData.json();
    return githubUsers;
  }
</script>

<Navbar isHome={false} />
<main class="content">
  {#await getGithubUsers()}
    <h2>Loading...</h2>
  {:then users}
    {#each users as user}
      <article class="user">
        <img src={user.avatar_url} alt={user.login} />
        <div class="user-info">
          <h3>User: {user.login}</h3>
          <a href={user.html_url} class="btn-primary" target="_blank">
            Github URL
          </a>
        </div>
      </article>
    {/each}
  {:catch error}
    <p>Something went wrong: {error}</p>
  {/await}
</main>
