<script>
  // libraries
  import { setContext, onMount, afterUpdate, onDestroy } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Total from "./Total.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";
  import Github from "./GithubUsers.svelte";

  // data
  //import ExpenseData from "./expenses";

  // variables
  let expenses = [];
  let setName = "";
  let setAmount = null;
  let setId = null;
  let isFormOpen = false;

  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((amount, item) => {
    return (amount += item.amount);
  }, 0);

  // functions
  const removeExpense = id => {
    expenses = expenses.filter(item => item.id !== id);
  };
  const clearExpenses = () => {
    expenses = [];
  };
  const addExpense = ({ name, amount }) => {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
    closeForm();
  };
  const setModifiedExpense = id => {
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  };
  const editExpense = ({ name, amount }) => {
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setAmount = null;
    setName = "";
    closeForm();
  };
  const showForm = () => {
    isFormOpen = true;
  };
  const closeForm = () => {
    isFormOpen = false;
    setId = null;
    setAmount = null;
    setName = "";
  };
  const setLocalStorage = () => {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  };
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  afterUpdate(() => {
    setLocalStorage();
  });

  // context
  setContext("removeExpense", removeExpense);
  setContext("addExpense", addExpense);
  setContext("modifyExpense", setModifiedExpense);
  setContext("editExpense", editExpense);
  setContext("closeForm", closeForm);
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm name={setName} amount={setAmount} {isEditing} />
    </Modal>
  {/if}
  <Total title="Total Expenses" {total} />
  <ExpensesList {expenses} />
  {#if expenses && expenses.length > 0}
    <button class="btn btn-primary btn-block" on:click={clearExpenses}>
      Clear Expenses
    </button>
  {/if}
</main>
