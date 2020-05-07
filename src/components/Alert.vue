<template>
  <div class="alert">
    <div class="popup">
      <div class="title">
        Потвердите действие
      </div>
      <div class="text">
        {{text}}
      </div>
      <div class="buttons">
        <div @click="no">
          Нет
        </div>
        <div @click="yes">
          Да
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
  props: {
    text: String,
  },
  methods: {
    yes(){
      this.$emit('result', true);
    },
    no(){
      this.$emit('result', false);
    },
  },
}
</script>

<style scoped="true">
.alert{
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  background-color: rgba(0, 0, 0, .5);
  z-index: 999;
  display: flex;
  align-items: center;
  justify-content: center;
}
.popup{
  width: 30%;
  height: auto;
  background-color: #FFF;
  border-radius: .2vw;
  box-shadow: 0 0 .3vw #333;
}
.title{
  width: 100%;
  padding: 1vw;
  border-bottom: .05vw solid #DDD;
}
.text{
  width: 100%;
  padding: 1vw;
  font-size: 0.9vw;
  color: #666;
}
.buttons{
  width: 100%;
  display: flex;
  flex-direction: row-reverse;
  padding: 1vw;
}
.buttons div{
  width: 5vw;
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
  margin-left: 1vw;
}
.buttons div:hover{
  cursor: pointer;
  opacity: .5;
}
</style>
