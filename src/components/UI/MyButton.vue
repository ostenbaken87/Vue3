<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name:'my-button'
}
</script>

<style scoped>
.btn {
  padding: 10px 10px;
  color: teal;
  font-weight: bold;
  background: none;
  border: 1px solid teal;
  border-radius: 5px;
}

.btn:hover {
  color: aliceblue;
  background-color: teal;
  cursor: pointer;
}
</style>