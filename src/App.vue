<template>
  <main id="">
    
    <div class="container">
      <header class="options">
        <h1>TODO</h1>
        <svg xmlns="http://www.w3.org/2000/svg" width="26" height="26"><path fill="#FFF" fill-rule="evenodd" d="M13 21a1 1 0 011 1v3a1 1 0 11-2 0v-3a1 1 0 011-1zm-5.657-2.343a1 1 0 010 1.414l-2.121 2.121a1 1 0 01-1.414-1.414l2.12-2.121a1 1 0 011.415 0zm12.728 0l2.121 2.121a1 1 0 01-1.414 1.414l-2.121-2.12a1 1 0 011.414-1.415zM13 8a5 5 0 110 10 5 5 0 010-10zm12 4a1 1 0 110 2h-3a1 1 0 110-2h3zM4 12a1 1 0 110 2H1a1 1 0 110-2h3zm18.192-8.192a1 1 0 010 1.414l-2.12 2.121a1 1 0 01-1.415-1.414l2.121-2.121a1 1 0 011.414 0zm-16.97 0l2.121 2.12A1 1 0 015.93 7.344L3.808 5.222a1 1 0 011.414-1.414zM13 0a1 1 0 011 1v3a1 1 0 11-2 0V1a1 1 0 011-1z"/></svg>
      </header>
      <div class="list_form">
          <div class="check_list">
            <svg xmlns="http://www.w3.org/2000/svg" width="11" height="9"><path fill="none" stroke="#FFF" stroke-width="2" d="M1 4.304L3.696 7l6-6"/></svg>
          </div>
          <form class="" @keydown.prevent.enter="addNewList">
            <input type="text" v-model="itemNew" ref="new" autofocus placeholder="What needs to be done">
          </form>
          <div class="delete_list">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
          </div>
        </div>


      
      <transition-group name="card" tag="ul">
        <li class="item_list" v-for="(item, index) in items" :key="index">
          <div class="check_list">
            <svg xmlns="http://www.w3.org/2000/svg" width="11" height="9"><path fill="none" stroke="#FFF" stroke-width="2" d="M1 4.304L3.696 7l6-6"/></svg>
          </div>
          
          <article class="text_list">
            {{ item.text }}
          </article>
          <div class="delete_list">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
          </div>
          
        </li>
      </transition-group>


      <footer class="items_info">
        <h1>5 items left</h1>
        <ul>
          <li>All</li>
          <li>Active</li>
          <li>Completed</li>
        </ul>
        <h1>
          clear all
        </h1>
      </footer>
    </div>
  </main>
</template>

<script>
import { reactive, toRefs, ref, onMounted } from 'vue'

export default {
  setup () {
    const root = ref(null)

    onMounted(() => {
      console.log(root.value)
    })

    const state = reactive({
      count: 0,
      itemNew: '',
      ref: '',
      items: [
        {
          id: 1,
          text: 'Something what need to be done'
        },
        {
          id: 2,
          text: 'Something what need to be done'
        },
        {
          id: 3,
          text: 'Something what need to be done'
        },
      ]
    })

    const addNewList = () => {
          state.items.push({
            text: state.itemNew
          })

          state.itemNew = ''

          
        }

        
    return {
      root,
      ...toRefs(state),
      addNewList
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
}
.options {
  display: flex;
  align-items: center;
}
.item_list {
  display: flex;
  align-items: center;
  border-bottom: 1px solid hsl(235, 19%, 35%);
  background-color: hsl(235, 24%, 19%);
}
.check_list {
  width: 10%;
  text-align: center;
  svg {
    padding: 0.5rem 0.5rem;
    border-radius: 100%;
    border: 1px solid white;
  }
  
}
.text_list {
  width: 80%;
}
.delete_list {
  width: 10%;
  text-align: center;
}
.items_info {
  text-align: center;
  font-size: 0.7rem;
  display: flex;
  align-items: center;
  background-color: hsl(235, 24%, 19%);
  h1 {
    width: 20%;
    font-size: 0.7rem;
    padding: 0 1rem;
  }
  ul {
    
    width: 60%;
    display: flex;
  }
  li {
    padding: 1rem 1rem;
  }
}
.list_form {
  display: flex;
  align-items: center;
  padding: 0.5rem 0;
  background-color: hsl(235, 24%, 19%);
  margin-bottom: 1.5rem;
  form {
    width: 100%;
  }
  input {
    color: white;
    font-size: 1.5rem;
    width: 95%;
    background-color: transparent;
    border: none;
    padding: 1rem 0.5rem;
  }
  .delete_list {
    width: 10%;
    padding: 0 0.5rem;
  }
  .check_list {
    padding: 0 0.5rem;
  }
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