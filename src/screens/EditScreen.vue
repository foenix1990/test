<template>
  <div class="content">
    <div class="input-block">
      <input type="text" v-model="item.title" class="input" maxlength="100" placeholder="Заголовок заметки" />
      <span>{{item.title.length}} / 100</span>
    </div>
    <div class="todos-add">
      <div class="button" @click="addTodo">
        Добавить Todo
      </div>
    </div>
    <div class="todos-list">
      <div class="todo" v-for="(todo, i) in item.todos" :key="i">
        <div>
          <span class="check">
            <input type="checkbox" v-model="todo.check" />
            <span>&#10004;</span>
          </span>
        </div>
        <div>
          <input type="text" class="input" v-model="todo.title" placeholder="Заголовок Todo" />
        </div>
        <div class="button" @click="removeTodo(i)">
          Удалить
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EditScreen',
  props: {
    id: Number,
    items: Array,
    save: Boolean,
  },
  data() {
    return {
      temp: null,
      item: {
        title: '',
        todos: [],
      },
    };
  },
  watch: {
    save(val) { /* Отслеживание кнопки сохранить */
      if (val === true) {
        this.saveItem();
      }
    },
  },
  methods: {
    addTodo() { /* Добавления Todo */
      const item = {
        title: '',
        check: false,
      };
      this.item.todos.push(item);
    },
    removeTodo(index) { /* Удаление Todo */
      this.item.todos.splice(index, 1);
    },
    saveItem() { /* Сохранение заметки */
      if (this.id !== null) {
        this.items[this.id] = this.item;
      } else {
        this.items.push(this.item);
      }
      localStorage.setItem('items', JSON.stringify(this.items)); /* Запись в локальное хранилище */
      this.$emit('edit', {value: false});
    },
  },
  mounted() {
    if (this.id !== null) { /* Проверка на измненения заметки */
      this.item = this.items[this.id];
    }
    const $this = this;
    document.addEventListener('keydown', function(event) { /* Сброс измненений */
      if (event.ctrlKey && event.key === 'z') {
        const items = JSON.parse(localStorage.getItem('items'));
        $this.temp = $this.item;
        items.forEach((item, index) => {
          if (index === $this.id) {
            $this.item = item;
          }
        });
      }
      if (event.ctrlKey && event.key === 'y') {
        $this.item = $this.temp;
      }
    });
  },
}
</script>

<style scoped="true">
/* Поле ввода */
.input-block{
  width: 100%;
}
.input{
  width: 100%;
  height: 2.3vw;
  border-radius: .2vw;
  border: .05vw solid #DDD;
  outline: none;
  padding: 0 1vw;
  padding-right: 2vw;
  font-size: 0.8vw;
}
.input-block span{
  position: absolute;
  top: 0;
  right: 1vw;
  height: 100%;
  display: flex;
  align-items: center;
  font-size: .8vw;
}
/* Кнопка добавления */
.todos-add{
  width: 100%;
  display: flex;
  flex-direction: row-reverse;
  padding: 1vw 0;
}
.button{
  width: 8vw;
  height: 2.3vw;
  font-size: .8vw;
  background: #EEE;
  border-radius: .2vw;
  border: .05vw solid #DDD;
  transition: .2s;
  display: flex;
  align-items: center;
  justify-content: center;
  user-select: none;
}
.button:hover{
  cursor: pointer;
  opacity: .5;
}
/* Список Todo */
.todos-list{
  width: 100%;
}
.todos-list .todo{
  width: 100%;
  display: flex;
  margin-bottom: 1vw;
}
.todos-list .todo div:nth-child(1){
  width: 2.3vw;
  flex-shrink: 0;
}
.todos-list .todo div:nth-child(2){
  width: 100%;
  padding: 0 1vw;
}
.todos-list .todo div:nth-child(3){
  flex-shrink: 0;
}
.check{
  width: 2.3vw;
  height: 2.3vw;
  display: flex;
  align-items: center;
  justify-content: center;
  border: .05vw solid #DDD;
  border-radius: .2vw;
}
.check input{
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0;
  cursor: pointer;
  z-index: 10;
}
.check span {
  color: transparent;
  transition: .2s;
}
.check input:checked + span{
  color: #444;
}
</style>
