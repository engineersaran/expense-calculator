<script>
  // libraries
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";

  // Components
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";

  //variable
  export let expenses = [];
</script>

<section>
  <SectionTitle title="Expenses List" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div
        in:fly={{ x: 200, delay: (index + 1) * 700 }}
        out:fly={{ x: -200 }}
        animate:flip>
        <Expense {...expense} />
      </div>
    {:else}
      <h3>No Expenses Added To The List!</h3>
    {/each}
  </ul>
</section>
