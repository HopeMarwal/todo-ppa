<template>
  <div class="wrapper">
    <div class="heading">
      <button @click="toggleAll" :data-completed="isCompleted">
        <i class="fa-solid fa-angle-down"></i>
      </button>
      <input
        type="text"
        placeholder="What needs to be done?"
        @keyup.enter="addToDoItem"
        v-model="newToDo"
      />
    </div>
    <div class="item" v-for="(todo, index) in filteredToDos" :key="index">
      <div class="container">
        <input :id="index" type="checkbox" v-model="todo.completed" />
        <span class="checkmark"></span>
      </div>
      <label :class="[{ done: todo.completed }]" :for="index">
        {{ todo.title }}
      </label>
      <button @click="deleteItem(index)">
        <i class="fa-solid fa-xmark"></i>
      </button>
    </div>
    <div class="footer" v-if="this.todos.length">
      <span>{{ activeTasks.length }} items left</span>
      <div class="footer__btns">
        <button
          @click="visibility = 'all'"
          :class="[{ selected: visibility === 'all' }]"
        >
          All
        </button>
        <button
          @click="visibility = 'active'"
          :class="[{ selected: visibility === 'active' }]"
        >
          Active
        </button>
        <button
          @click="visibility = 'completed'"
          :class="[{ selected: visibility === 'completed' }]"
        >
          Completed
        </button>
      </div>
      <span class="remove" @click="removeCompleted">Clear completed</span>
    </div>
  </div>
</template>

<script>
/*eslint-disable */
export default {
  name: 'IndexPage',
  data() {
    return {
      todos: [
        {
          title: 'item1',
          completed: false
        },
        {
          title: 'item2',
          completed: false
        },
        
      ],
      newToDo: '',
      visibility: 'all',
      isCompleted: false
    }
  },
  methods: {
    addToDoItem() {
      let value = this.newToDo.trim()

      let toDoItem = {
        title: value,
        completed: false
      }

      if(!value) {
        this.newToDo = ''
        return
      }

      this.todos.push(toDoItem)
      this.newToDo = ''
    },

    deleteItem(index) {
      this.todos.splice(index, 1)
      
    },

    toggleAll() {
      this.isCompleted = !this.isCompleted
      this.todos.forEach(todo => {
        todo.completed = this.isCompleted
      });
    },

    removeCompleted() {
      this.todos = this.todos.filter(item => {
        return !item.completed
      })
    }
  },
  computed: {
    activeTasks() {
      return this.todos.filter(todo => !todo.completed)
    },
    filteredToDos() {
      if (this.visibility === 'all') {
        return this.todos
      } else if (this.visibility === 'active') {
        return this.activeTasks
      } else {
        return this.todos.filter(todo => todo.completed)
      }
    }
  }
}
</script>

<style lang="scss">
 
  @import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.8.1/font/bootstrap-icons.css");
  .wrapper {
    max-width: 500px;
    margin: auto;
    background: #fff;
    box-shadow: 0px 0px 15px 5px rgba(173, 173, 173, 0.733);
    button {
      background-color: transparent;
      border: none;
      cursor: pointer;
      &:focus {
        outline: none;
        border: none;
      }
    }
    .heading {
      padding: 20px 16px;
      background-color: rgba(242, 242, 242, 0.488);
      border-bottom: 2px solid rgba(204, 204, 204, 0.783);
      button {
        padding: 5px 5px 5px 0;
        font-size: 20px;
      }
    }
    input[type=text] {
      background-color: transparent;
      border: none;
      width: 90%;
      font-size: 20px;
      &::placeholder {
        font-weight: 200;
        font-style: italic;
        opacity: 0.5;
      }
      &:focus {
        outline: none;
        border: none;
      }
    }
    .item {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      padding: 10px 5px;
      background-color: rgba(242, 242, 242, 0.2);
      border-bottom: 1px solid rgba(204, 204, 204, 0.529);
      &:hover {
        button {
          opacity: 1;
        }
      }
      label {
        font-size: 20px;
        letter-spacing: 0.05em;
        text-transform: capitalize;
        &.done {
          text-decoration: line-through;
          opacity: 0.3;
        }
      }
      .container {
        position: relative;
        width: 30px;
        height: 30px;
        cursor: pointer;
        user-select: none;
      }
      input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 30px;
        width: 30px;
        z-index: 1;
        &:checked {
          ~ .checkmark {
            &:after {
              display: block;
            }
          }
        }
      }
      .checkmark {
        position: absolute;
        top: 0;
        left: 0;
        height: 30px;
        width: 30px;
        border: 1px solid #ccc;
        border-radius: 50%;
        &:after {
          content: "";
          position: absolute;
          display: none;
          left: 11px;
          top:3px;
          width: 6px;
          height: 16px;
          border: solid rgb(29, 164, 34);
          border-width: 0 2px 2px 0;
          -webkit-transform: rotate(45deg);
          -ms-transform: rotate(45deg);
          transform: rotate(45deg);
        }
      }
      button {
        font-size: 16px;
        font-weight: 400;
        padding-right: 10px;
        color: rgba(92, 20, 20, 0.424);
        opacity: 0;
      }
    }
    .footer {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      padding: 5px 10px;
      &__btns {
        button {
          padding: 3px 6px;
          font-size: 12px;
          font-weight: 600;
          color: rgba(97, 96, 96, 0.863);
          border: 1px solid transparent;
          transition: all 0.3s linear;
          border-radius: 3px;
          &.selected {
            border-color: rgba(161, 161, 161, 0.404);
            
          }
        }
      }
      span {
        font-size: 12px;
        font-weight: 500;
        color: rgba(85, 85, 85, 0.863);
        &.remove {
          cursor: pointer;
        }
      }
    }
  }
</style>