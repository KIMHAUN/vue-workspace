<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keyup.enter="addTodo">
    <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"/>
    </span>
    <!-- <button v-on:click="addTodo">추가</button> -->
    <modal v-if="showModal" @close="showModal = false">
      <h3>경고</h3>
      <span slot="footer" @click="showModal = false">
        할 일을 입력하세요
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
    </modal>

    <Teleport to="body">
    <!-- use the modal component, pass in the prop -->
    <modal :show="showModal" @close="showModal = false">
      <template #header>
        <h3>경고</h3>
      </template>
      <template #footer >
        <h3>할 일을 입력하세요</h3>
        <i class="closeModalBtn fas fa-times" aria-hidden="true" @click="showModal = false"></i>
      </template>
    </modal>
  </Teleport>
  </div>
</template>

<script>
import Modal from './common/Modal.vue'

export default {
  components: {
    Modal: Modal
  },
  props: ['propsdata'],
  data() {
    return {
      newTodoItem: '',
      showModal: false
    }
  },
  methods: {
    addTodo() {
      //console.log(this.newTodoItem)
      if (this.newTodoItem !== "") {
        var value = this.newTodoItem && this.newTodoItem.trim();
        //localStorage.setItem(this.newTodoItem, this.newTodoItem);
        this.$emit('addTodo', value);
        this.clearInput();
      } else {
        this.showModal = !this.showModal;
        console.log(this.showModal);
        console.log("empty");
      }
    },
    clearInput() {
      this.newTodoItem = '';
    }
  }
}
</script>

<style scoped>
  input:focus {
    outline: none;
  }
  .inputBox {
    background: white;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
</style>
