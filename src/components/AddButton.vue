<template>
  <button class="add__btn" @click="$emit('openModal')">
    <img src="@/assets/img/pencil.svg" alt="" />
  </button>
</template>

<script>
export default {};
</script>

<style>
.add__btn {
  box-shadow: 0px 4px 8px 3px rgba(0, 0, 0, 0.15);
  filter: drop-shadow(0px 1px 3px rgba(0, 0, 0, 0.3));
  padding: 16px;
  background: linear-gradient(0deg, rgba(103, 80, 164, 0.11), rgba(103, 80, 164, 0.11)), #FFFBFE;
  border-radius: 16px;
  position: fixed;
  right: 30px;
  bottom: 30px;
}
</style>