<script setup lang="ts">
const reload = () => {
  location.reload;
};
</script>

<template>
  <div class="header">
    <img
      @click="reload"
      src="@/components/icons/cashflow-icon.svg"
      alt="Logo"
    />
    <h1><span>Cash</span>Flow</h1>
    <span></span>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100vw;
}
img,
span {
  width: 48px;
}
h1 {
  font-size: 1.4rem;
  color: var(--brand-blue);
}
h1 span {
  color: var(--brand-green);
}
</style>
