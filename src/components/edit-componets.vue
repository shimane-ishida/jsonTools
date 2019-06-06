<template>
  <div>
    編集用コンポーネント
  </div>
</template>

<script>

export default {
  props: {
    jsonData: Object
  },
  created() {
    this.$on("send-data", this.jsonData);
    console.log('send-data',this.jsonData);
    
  }
}
</script>