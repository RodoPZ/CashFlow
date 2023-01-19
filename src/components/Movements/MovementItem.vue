<script setup lang="ts">
import { toRefs, computed } from "vue";
import { numberToCurrency } from "@/utils/numberToCurrency";
const props = defineProps({
  title: String,
  description: String,
  id: Number,
  amount: Number,
});

const { title, id, description, amount } = toRefs(props);
const amountCurrency = computed(() => {
  return numberToCurrency.format(amount!.value!);
});
const emit = defineEmits(["remove"]);
const remove = () => emit("remove", id!.value);
const isNegative = computed(() => amount!.value! < 0);
</script>

<template>
  <div class="movement">
    <div class="content">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img
        src="@/components/icons/trash-icon.svg"
        alt="borrar"
        @click="remove"
      />
      <p :class="[isNegative ? 'red' : 'green']">{{ amountCurrency }}</p>
    </div>
  </div>
</template>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
  color: red;
}
.green {
  color: green;
}
</style>
