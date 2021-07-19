<template>
  <div class="modal-bg" @click.self="closeModal">
    <form
      class="add-expense-form"
      @submit.prevent="submit"
    >
      <fieldset>
        <label for="amount" :class="{error: errors.amount}">Amount:</label>
        <input
          type="number"
          name="amount"
          id="amount"
          v-model="amount"
          step="0.01"
          min="0"
          :class="{error : errors.amount}"
          @input="validateAmount"
        >
        <p class="error-text" v-if="errors.amount">{{errors.amount}}</p>
      </fieldset>
      <fieldset>
        <label for="category" :class="{error : errors.category}">Category:</label>
        <select
          name="categories"
          id="categories"
          v-model="category"
          :class="{error : errors.category}"
          @change="validateCategory"
        >
          <option
            v-for="option in categoryOptions"
            :key="option"
            :value="option"
          >
            {{option}}
          </option>
        </select>
        <p class="error-text" v-if="errors.category">{{errors.category}}</p>
      </fieldset>
      <fieldset>
        <label for="comment">Comment:</label>
        <textarea name="comment" id="comment" cols="20" rows="5" v-model="comment"/>
      </fieldset>
      <input type="submit" class="btn submit-btn" value="Add">
    </form>
  </div>
</template>

<script>
export default {
  name: 'ExpenseForm',
  props: {
    balance: Number,
  },
  data: () => ({
    categoryOptions: [
      'Housing',
      'Food',
      'Insurance',
      'Transportation',
      'Taxes',
    ],
    amount: 0,
    category: null,
    comment: '',
    errors: {
      amount: false,
      category: false,
    },
  }),
  methods: {
    submit() {
      if (this.validate() > 0) return;
      this.$emit('submit', {
        amount: this.amount,
        category: this.category,
        comment: this.comment,
      });
      this.resetExpenseForm();
    },
    validate() {
      this.validateAmount();
      this.validateCategory();
      const errors = Object.keys(this.errors).filter((key) => this.errors[key]);
      return errors.length;
    },
    validateAmount() {
      this.errors.amount = false;
      const amount = parseFloat(this.amount, 10);
      if (Number.isNaN(amount) || amount <= 0 || amount > this.balance) {
        this.errors.amount = 'Invalid amount';
      }
    },
    validateCategory() {
      this.errors.category = false;
      if (!this.category) {
        this.errors.category = 'Please choose a category';
      }
    },
    resetExpenseForm() {
      this.amount = 0;
      this.category = null;
      this.comment = '';
    },
    closeModal() {
      this.$emit('close');
      this.resetExpenseForm();
    },
  },
};
</script>

<style scoped lang="sass">
.modal-bg
  width: 100%
  height: 100%
  position: absolute
  left: 0
  top: 0
  background-color: rgba(0,0,0,0.3)
  display: flex
  .add-expense-form
    margin: auto
    background-color: #fff
    min-width: 25rem
    padding: 1rem
    border-radius: 5px
    fieldset
      border: none
      padding: 1rem
      display: flex
      justify-content: space-between
      position: relative
      label
        width: 25%
      input,
      select,
      textarea
        box-sizing: border-box
        flex-grow: 1
        background: white
        border: 1px solid grey
        border-radius: 6px
        padding: 14px 0 14px 20px
        font-size: 1.125rem
        line-height: 1.875rem
        color: black
        margin-bottom: 0
        &::placeholder
          font-size: 1.125rem
          line-height: 1.875rem
          color: black
        &:active,
        &:hover
          box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.08)
      textarea
        padding: 14px
      label.error
        color: red
      input.error,
      select.error
        border: 1px solid red
      .error-text
        position: absolute
        bottom: -1rem
        left: 30%
        font-size: 0.75em
        color: red

    .submit-btn
      width: 100%
      cursor: pointer
      user-select: none
      border: none
      color: white
      padding: 1rem
      border-radius: 4px
      background-color: #3e8e41
      &:active
        transform: translateY(1px)
</style>
