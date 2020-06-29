<script>
  // libraries
  import { getContext } from "svelte";

  // components
  import Title from "./Title.svelte";

  // variables
  export let name = "";
  export let amount = null;
  export let isEditing;

  // reactive
  $: isEmpty = !name || !amount;

  // context
  const addExpense = getContext("addExpense");
  const editExpense = getContext("editExpense");
  const closeForm = getContext("closeForm");

  // functions
  const handleSubmit = () => {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }
    name = "";
    amount = null;
  };
</script>

<section class="form">
  {#if isEditing}
    <Title title="Edit Expense" />
  {:else}
    <Title title="Add Expense" />
  {/if}
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="Name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="Amount">Amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}
    <button
      class="btn btn-block"
      class:disabled={isEmpty}
      type="submit"
      disabled={isEmpty}>
      {#if isEditing}edit expense{:else}add expense{/if}
    </button>
    <button class="close-btn" type="button" on:click={closeForm}>
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
