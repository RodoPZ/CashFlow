<script setup lang="ts">
import { computed } from "vue";
import { numberToCurrency } from "@/utils/numberToCurrency";

const props = defineProps({
  label: {
    type: String,
    default: null,
  },
  amount: {
    type: Number,
    default: null,
  },
});

const amountCurrency = computed(() => numberToCurrency.format(props.amount));
</script>

<template>
  <main>
    <p>{{ props.label }}</p>
    <h1>{{ amountCurrency }}</h1>
    <div class="graphic">
      <slot name="graphic"></slot>
    </div>
    <div class="action">
      <slot name="action"></slot>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>
