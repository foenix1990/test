<template>
  <div id="app">
    <Alert :text="popupText" v-on:result="popupEvent" v-if="popup" />
    <div class="header">
      <div class="container">
        <span>Заметки</span>
        <Buttons :edit="edit" v-on:edit="editFunc" v-on:save="save = true" />
      </div>
    </div>
    <div class="body">
      <IndexScreen v-if="!edit" :items="items" v-on:edit="editFunc" v-on:remove="removeFunc" />
      <EditScreen v-if="edit" :items="items" :id="editId" :edit="edit" :save="save" v-on:edit="editFunc" />
    </div>
  </div>
</template>

<script>
import Alert from './components/Alert.vue'; /* Компонент всплывающего окна */
import Buttons from './components/Buttons.vue'; /* Компонент кнопок */
import IndexScreen from './screens/IndexScreen.vue'; /* Страница списка */
import EditScreen from './screens/EditScreen.vue'; /* Страница измненения */

export default {
  name: 'App',
  components: {
    Alert,
    Buttons,
    IndexScreen,
    EditScreen,
  },
  data() {
    return {
      popup: false,
      popupText: '',
      save: false,
      edit: false,
      editId: null,
      items: [],
    };
  },
  methods: {
    editFunc(val) { /* Изменения заметки */
      this.save = false;
      if (val.value === 1) { /* Проверка на кнопку добавить и изменить */
        if (val.id || (val.id === 0)) {
          this.editId = val.id;
        } else {
          this.editId = null;
        }
        this.edit = true;
      } else if (val.value === 2) { /* Проверка на кнопку отмены */
        this.popupText = 'Вы уверены что хотите отменить изменения?';
        this.popup = true;
      } else {
        this.edit = false; /* Кнопку сохранить */
      }
    },
    removeFunc(val) { /* Удаления заметки */
      this.popupText = 'Вы уверены что хотите удалить заметку?';
      this.popup = true;
      this.editId = val;
    },
    popupEvent(val) { /* События всплывающего окна */
      if (this.editId !== null && val === true) {
        if (this.items[this.editId]) {
          this.items.splice(this.editId, 1);
          localStorage.setItem('items', JSON.stringify(this.items));
        }
      }
      if (val === true) {
        this.edit = false;
      }
      this.popup = false;
      this.editId = null;
    }
  },
  created() {
    if (localStorage.getItem('items')) {
      this.items = JSON.parse(localStorage.getItem('items')); /* Чтение из локального хранилища */
    }
  },
}
</script>

<style>
body {
  font-family: Arial;
  font-size: 1vw;
  margin: 0;
  padding: 0;
}
* {
  box-sizing: border-box;
  position: relative;
}
.header{
  height: 2.8vw;
  border-bottom: .05vw solid #DDD;
}
.header .container{
  width: 60%;
  height: 100%;
  margin: 0 auto;
  display: flex;
  align-items: center;
  border-left: .05vw solid #DDD;
}
.header .container span{
  font-size: 1vw;
  font-weight: 650;
  color: #555;
  padding-left: 1vw;
}
.body{
  width: 60%;
  margin: 0 auto;
}
.content{
  width: 100%;
  padding: 3vw 0;
}

</style>
