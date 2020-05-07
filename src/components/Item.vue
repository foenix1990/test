<template>
  <div class="item">
    <div class="title">
      <div>{{item.title}}</div>
      <div @click="editFunc(1)">&#9998;</div>
      <div @click="removeItem()">&#10006;</div>
    </div>
    <div class="todos" v-if="item.todos.length">
      <div v-for="(todo, i) in item.todos" :key="i" :class="[todo.check ? 'line-through' : '']">
        {{todo.title}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
  props: {
    item: Object,
    index: Number,
  },
  methods: {
    editFunc(val) { /* Изменения заметки */
      this.$emit('edit', {value: val, id: this.index});
    },
    removeItem() { /* Удаления заметки */
      this.$emit('remove', this.index);
    },
  },
}
</script>

<style scoped="true">
.item{
  width: 100%;
  background-color: #E8E8E8;
  border-radius: .2vw;
  overflow: hidden;
  margin-bottom: 1vw;
}
.title{
  width: 100%;
  height: 2.5vw;
  display: flex;
  align-items: center;
  padding-left: 1vw;
  padding-right: .5vw;
}
.title div{
  width: 100%;
}
.title div:nth-child(2),
.title div:nth-child(3) {
  width: 2.5vw;
  flex-shrink: 0;
  text-align: center;
}
.title div:nth-child(2):hover,
.title div:nth-child(3):hover {
  opacity: .5;
  cursor: pointer;
}
.todos{
  width: 100%;
  padding: 1vw;
  background: #F8F8F8;
}
.todos div{
  padding: .5vw;
  font-size: 0.9vw;
}
.todos div.line-through{
  text-decoration: line-through;
}
</style>
