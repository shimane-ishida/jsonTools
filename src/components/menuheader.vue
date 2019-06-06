<template>
  <div class="myheader">
    <h3>JSON形式設定</h3>
    <label for="addElement">要素名：</label>
    <input type="text" id="addElement" v-model="element">
    <input type="button" value="追加" @click="addElement">
    <input type="button" value="初期化" @click="clearElement">
    <p>
      現在設定中のJSON形式
    </p>
    <div class="jsonarea">
      {{ jsonModel }}
    </div>
  </div>
</template>

<script>
export default {
  mounted(){
    this.init();
  },
  data(){
    return{
      jsonModel: {},
      element: "",
    }
  },
  methods: {
      addElement()
      {
        if(this.element === "") return;
        this.$set(this.jsonModel, this.element, "");
        this.element = "";
        //親要素へ通知
        this.$emit('child-event', this.jsonModel);
      },
      clearElement()
      {
        if(!confirm("現在設定中の形式を初期化してよろしいですか？")) return;
        this.init();
      },
      init: function(){
        this.jsonModel = {
          "title": "",
          "lat": "0.0",
          "lng": "0.0",
        }
        this.$emit('child-event', this.jsonModel);
      }
  },
}
</script>
<style>
.myheader {
  background: beige;
  padding-left: 5px;
  padding-bottom: 5px;
}
.myheader > h3 {
  padding-top:5px; 
}
.myheader > input[type="text"]{
  padding: 3px;
}
.jsonarea{
  background: rgb(192, 185, 185);
  padding: 5px;
}
input[type="button"]{
  margin: 5px;
}
</style>