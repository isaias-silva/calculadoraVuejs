<template>
  <button>
    {{symbol}}
  </button>
</template>

<script lang="ts">

export default {
  name: 'buttonB',
  props: {
    symbol: String,
    title: String
  }
}
</script>
<style>
button{
  font-size: 18px;
  border: 1px solid green;
  border-radius: 10px;
  transition: linear 0.2s;
}
button:hover{
  cursor: pointer;
  background-color: green;
  color: #fff;
}
</style>
