<template>
  <main id="" :class="{ dark: dark == true, light: dark == false }">
    
    <div class="container">
      <header class="options">
        <h1>TODO</h1>
        <a @click.prevent="toggleTheme()" href="">
          <svg xmlns="http://www.w3.org/2000/svg" width="26" height="26"><path fill="#FFF" fill-rule="evenodd" d="M13 21a1 1 0 011 1v3a1 1 0 11-2 0v-3a1 1 0 011-1zm-5.657-2.343a1 1 0 010 1.414l-2.121 2.121a1 1 0 01-1.414-1.414l2.12-2.121a1 1 0 011.415 0zm12.728 0l2.121 2.121a1 1 0 01-1.414 1.414l-2.121-2.12a1 1 0 011.414-1.415zM13 8a5 5 0 110 10 5 5 0 010-10zm12 4a1 1 0 110 2h-3a1 1 0 110-2h3zM4 12a1 1 0 110 2H1a1 1 0 110-2h3zm18.192-8.192a1 1 0 010 1.414l-2.12 2.121a1 1 0 01-1.415-1.414l2.121-2.121a1 1 0 011.414 0zm-16.97 0l2.121 2.12A1 1 0 015.93 7.344L3.808 5.222a1 1 0 011.414-1.414zM13 0a1 1 0 011 1v3a1 1 0 11-2 0V1a1 1 0 011-1z"/></svg>
        </a>
        
      </header>
      <div class="list_form">
          <div class="check_list">
            <svg xmlns="http://www.w3.org/2000/svg" width="11" height="9"><path fill="none" stroke="#FFF" stroke-width="2" d="M1 4.304L3.696 7l6-6"/></svg>
          </div>
          <div class="" @keydown.prevent.enter="addNewList">
            <input type="text" v-model="itemNew" ref="new" autofocus placeholder="What needs to be done">
          </div>
          <div class="delete_list">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
          </div>
        </div>


      
      <transition-group name="card" tag="ul">
        <li class="item_list" v-for="(item, index) in itemsFiltered" :key="item.id">
          <div class="check_list">
            <a 
              @click.prevent="addCompleted(item)"  
              :class="{ completed: item.completed == true, uncompleted: item.completed == false }"
              href=""
            >
              <svg  xmlns="http://www.w3.org/2000/svg" width="11" height="9"><path fill="none" stroke="#FFF" stroke-width="2" d="M1 4.304L3.696 7l6-6"/></svg>
            </a>
            
          </div>
          
          <article class="text_list"
            :class="{ completed: item.completed == true, uncompleted: item.completed == false }"
          >
            {{ item.text }}
          </article>
          <div class="delete_list">
            <a @click.prevent="removeList(index)" href="">
              <div class="delete_list">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
              </div>
            </a>
            
          </div>
          
        </li>
      </transition-group>


      <footer class="items_info">
        <h1>{{ remaining }}</h1>
        
        <ul>
          <li :class="{ active: filter == 'all' }" @click="filter = 'all'">All</li>
          <li :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</li>
          <li :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</li>
        </ul>
        <h1 v-if="items.filter(item => item.completed).length" class="clearButton" @click="clearCompleted()">
          clear completed
        </h1>
      </footer>
    </div>
  </main>
</template>

<script>
import { reactive, toRefs, ref, computed } from 'vue'

export default {
  setup () {
    const root = ref(null)

    

    const state = reactive({
      itemNew: '',
      filter: 'all',
      dark: true,
      items: [
        {
          id: 1,
          text: 'Everything id done for now',
          completed: true
        },
        {
          id: 2,
          text: 'Something what need to be done',
          completed: false
        },
        {
          id: 3,
          text: 'Noone is safe today',
          completed: false
        },
      ]
    })


    
    const itemsFiltered = computed(() => {
      if (state.filter == 'all') {
        return state.items;
      } else if (state.filter == 'active') {
        return state.items.filter(item => !item.completed);
      } else if (state.filter == 'completed') {
        return state.items.filter(item => item.completed);
      }
      return this.items
    })
    
    const remaining = computed(() => {
      const sum = state.items.filter(item => !item.completed).length;
      if (state.items.filter(item => !item.completed).length <= 1) {
        return sum + ' item left'
      } else {
        return sum + ' items left'
      }
      
    })

    const toggleTheme = () => {
      state.dark = !state.dark;
    }

    const addNewList = () => {
        let itemMaxId = state.items.length
          state.items.push({
            id: itemMaxId + 1,
            text: state.itemNew,
            completed: false
          })
          state.itemNew = ''
        }
    const removeList = (index) => {
      state.items.splice(index, 1)
    }

    const addCompleted = (item) => {
      item.completed = !item.completed;
      
    }

    const remainingAll = () => {
      return state.items.filter(item => !item.completed).length;
      
    }

    const clearCompleted = () => {
      state.items = state.items.filter(item => !item.completed)
    }

    
        
    return {
      root,
      ...toRefs(state),
      addNewList,
      removeList,
      remaining,
      addCompleted,
      remainingAll,
      itemsFiltered,
      clearCompleted,
      toggleTheme
    }
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
  color: hsl(236, 33%, 92%);
  font-size: 18px;
}
ul {
  margin: 0;
  padding-left: 0;
  list-style: none;
}
ul li {
  padding: 1rem 0;

}
main {
  width: 100vw;
  height: 100vh;
  background-color: hsl(235, 21%, 11%);
  background-image: url('./assets/images/bg-desktop-dark.jpg');
  background-repeat: no-repeat;
  background-size: contain;
  display: flex;
  align-items: center;
}
.container {
  width: 40%;
  height: auto;
  margin: 0 auto; 
  svg path {
    stroke: transparent;
  }
  
}
.options {
  padding: 0 0.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.item_list {
  display: flex;
  align-items: center;
  border-bottom: 1px solid hsl(235, 19%, 35%);
  background-color: hsl(235, 24%, 19%);
}
.item_list:first-child {
  border-top-left-radius: 0.5rem;
  border-top-right-radius: 0.5rem;
}

.item_list:hover .delete_list svg path {
  fill: hsl(235, 19%, 35%);
}
.check_list {
  width: 10%;
  text-align: center;
  svg {
    padding: 0.5rem 0.5rem;
    border-radius: 100%;
    border: 1px solid hsl(235, 19%, 35%);
    cursor: pointer;
  }
  svg:hover {
    border: 1px solid #fff;
  }
  .completed {
    svg {
      background: linear-gradient( hsl(192, 100%, 67%), hsl(280, 87%, 65%));
    }
    svg path {
      stroke: #fff;
    }
  }
}
.text_list {
  width: 80%;
  
}
.completed {
    color: hsl(235, 19%, 35%);
    text-decoration: line-through;
  }
.delete_list {
  width: 10%;
  text-align: center;
  svg path {
    fill: transparent;
  }
  
}
.items_info {
  border-bottom-left-radius: 0.5rem;
  border-bottom-right-radius: 0.5rem;
  text-align: center;
  font-size: 0.7rem;
  display: flex;
  align-items: center;
  background-color: hsl(235, 24%, 19%);
  h1 {
    width: 20%;
    font-size: 0.7rem;
    padding: 0 1rem;
    opacity: 0.5;
  }
  
  ul {
    cursor: pointer;
    width: 60%;
    display: flex;
  }
  li {
    opacity: 0.5;
    padding: 1rem 1rem;
  }
  li:hover {
    opacity: 1;
  }
  ul .active {
    opacity: 1;
    color: hsl(220, 98%, 61%);
  }
}
.list_form {
  display: flex;
  align-items: center;
  padding: 0.5rem 0;
  background-color: hsl(235, 24%, 19%);
  margin-bottom: 1.5rem;
  border-radius: 0.5rem;
  
  form {
    width: 100%;
  }
  input {
    color: white;
    font-size: 1rem;
    width: 95%;
    background-color: transparent;
    border: none;
    padding: 1rem 0;
  }
  .delete_list {
    width: 10%;
    padding: 0 0.5rem;
  }
  
  input:focus-visible {
    outline: 0px;
  }
}
.clearButton {
  cursor: pointer;
  opacity: 0.5;
}
.clearButton:hover {
  opacity: 1;
}
.form_circle {
  width: 20%;
  span {
    width: 200px;
    height: 20px;
    border: 1px solid white;
    border-radius: 100%;
  }
}
.card-enter-active,
.card-leave-active {
  transition: all 0.3s;
}

.card-enter-from,
.card-leave-to {
  opacity: 0;
  transform: scale(0.75);
}
</style>