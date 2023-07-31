<template>
  <div class="navbar">
    <div @click="$router.push('/')" class="navbar__title">Vue 3</div>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')" class="navbar__btn">Posts</my-button>
      <my-button @click="$router.push('/about')" class="navbar__btn">About</my-button>
    </div>
  </div>
</template>

<script>


export default {


}
</script>

<style scoped>
.navbar {
  height: 50px;
  background: lightgrey;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 20px;
}

.navbar__btns {
  margin-left: auto;
}

.navbar__btn {
  margin-right: 10px;
}

.navbar__title {
  font-weight: bold;
  font-size: 30px;
  color: teal;
  cursor: pointer;
}
</style>