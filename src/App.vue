<template>
  <div id="app">
    <h1>Total balance: ${{ balance }}</h1>
    <button
      class="btn add-btn"
      @click="showExpenseForm"
      v-if="balance > 0"
    >
      Add expense
    </button>
    <table class="expenses">
      <thead>
        <tr>
          <th>Amount</th>
          <th>Category</th>
          <th>Comment</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="expense, index in expenses" :key="`${index}_${expense.amount}`">
          <td>${{expense.amount}}</td>
          <td>{{ expense.category }}</td>
          <td>{{ expense.comment }}</td>
        </tr>
      </tbody>
    </table>
    <expense-form
      v-if="isExpenseFormShown"
      @submit="onFormSubmit"
      @close="hideExpenseForm"
      :balance="balance"
    />
  </div>
</template>

<script>
import ExpenseForm from '@/components/ExpenseForm.vue';

export default {
  name: 'App',
  components: {
    ExpenseForm,
  },
  data: () => ({
    balance: 1000,
    isExpenseFormShown: false,
    expenses: [],
  }),
  methods: {
    showExpenseForm() {
      this.isExpenseFormShown = true;
    },
    hideExpenseForm() {
      this.isExpenseFormShown = false;
    },
    onFormSubmit(obj) {
      this.addExpense(obj);
      this.hideExpenseForm();
    },
    addExpense(obj) {
      const { amount, category, comment } = obj;
      this.expenses.push({ amount, category, comment });
      this.balance = Math.round((this.balance - amount) * 100) / 100;
    },
  },
};
</script>

<style lang="sass">
  h1,h2,h3,h4,
  p,div,label,span,
  input,textarea,select,option
    font-family: sans-serif
  html, body
    width: 100%
    height: 100%
    margin: 0
    padding: 0
  #app
    width: 100%
    height: 100%
    display: flex
    flex-direction: column
    align-items: center
    padding: 5rem 0
    .add-btn
      cursor: pointer
      user-select: none
      border: none
      color: white
      padding: 1rem
      border-radius: 4px
      background-color: #3e8e41
      &:active
        transform: translateY(1px)
    .expenses
      width: 60%
      margin-top: 2rem
      border-collapse: collapse
      th
        text-align: center
        border-bottom: 1px solid black
        padding: 1rem 0
      td
        text-align: center
        border-bottom: 1px solid black
        width: 33%
        padding: 1rem 0
</style>
