<template>
  <div class="content">
    <Item v-for="(item, i) in items" :key="i" :item="item" v-on:edit="editFunc" :index="i" v-on:remove="removeItem" />
  </div>
</template>

<script>
import Item from '../components/Item.vue'; /* Компонент заметки */

export default {
  name: 'Index',
  components: {
    Item,
  },
  props: {
    items: Array,
  },
  data() {
    return {
      id: null,
    };
  },
  methods: {
    editFunc(val) { /* Изменения заметки */
      this.$emit('edit', val);
    },
    removeItem(val) { /* Удаления заметки */
      this.$emit('remove', val);
    },
  },
}
</script>

<style scoped="true">

</style>
