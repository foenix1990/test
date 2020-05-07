<template>
  <div class="buttons-content">
    <div class="button" v-if="!edit" @click="editFunc(1)">
      Добавить
    </div>
    <div class="button" v-if="edit" @click="editFunc(2)">
      Отменить
    </div>
    <div class="button" v-if="edit" @click="saveFunc(3)">
      Сохранить
    </div>
  </div>
</template>

<script>
export default {
  name: 'EditScreen',
  props: {
    edit: Boolean,
  },
  methods: {
    editFunc(val) { /* Функция добавить и изменить */
      this.$emit('edit', {value: val});
    },
    saveFunc(val) { /* Функция сохранить */
      this.$emit('save', val);
    },
  },
}
</script>

<style scoped="true">
.buttons-content{
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row-reverse;
  border-right: .05vw solid #DDD;
}
.button{
  width: 8vw;
  text-align: center;
  padding: .5vw 0;
  border-radius: .2vw;
  font-size: .7vw;
  transition: .2s;
  color: #444;
  border-left: 0.05vw solid #DDD;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
  letter-spacing: .05vw;
}
.button:hover{
  cursor: pointer;
  opacity: .5;
}
</style>
