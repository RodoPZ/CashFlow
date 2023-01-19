<script setup lang="ts">
import LayoutView from "./LayoutView.vue";
import HeaderView from "./HeaderView.vue";
import ResumeView from "./Resume/ResumeView.vue";
import MovementsView from "./Movements/MovementsView.vue";
import ActionViewVue from "./ActionView.vue";
import GraphicView from "./Resume/GraphicView.vue";
import { computed, onMounted, ref, type Ref } from "vue";
import type { MovementTypes } from "@/components/models/Movement.model";
let movementlist: Ref<MovementTypes[]> = ref([]);
const localStorageName = "movements";
let tapped = ref(false);
let specificAmount: Ref<number> = ref(0);
let specificDate: Ref<string> = ref("");
const amounts = computed(() => {
  const lastDays = movementlist.value
    .filter((m) => {
      const today: Date = new Date();
      const oldDate = new Date(today.setDate(today.getDate() - 30));
      const mDate = new Date(m.time);
      return mDate > oldDate;
    })
    .map((m) => m.amount);
  return lastDays.map((m, index) => {
    const lastMovements = lastDays.slice(0, index + 1);

    return lastMovements.reduce((suma, movement) => {
      return suma + movement;
    }, 0);
  });
});

const labelDisplayed = computed(() => {
  if (tapped.value == true) {
    return specificDate.value;
  } else {
    return "Ahorro total";
  }
});

const amountDisplayed = computed(() => {
  if (tapped.value == true) {
    return specificAmount.value;
  } else {
    return movementlist.value.reduce((suma, m) => {
      return suma + m.amount;
    }, 0);
  }
});

onMounted(() => {
  const movements = localStorage.getItem(localStorageName);

  if (movements) {
    movementlist.value = JSON.parse(movements);
  }
});

const create = (movement: MovementTypes) => {
  movementlist.value.push(movement);
  console.log(movementlist);
  save();
};
const save = () => {
  localStorage.setItem(localStorageName, JSON.stringify(movementlist.value));
};
const remove = (id: number) => {
  movementlist.value = movementlist.value.filter((m) => m.id != id);
  save();
};
const select = (el: Array<number>) => {
  tapped.value = true;
  specificAmount.value = el[0];
  specificDate.value = new Date(
    movementlist.value[el[1]].time
  ).toLocaleDateString();
};
const deselected = () => {
  tapped.value = false;
};
</script>

<template>
  <LayoutView>
    <template #header>
      <HeaderView />
    </template>
    <template #resume>
      <ResumeView :label="labelDisplayed" :amount="amountDisplayed">
        <template #graphic>
          <GraphicView
            :amounts="amounts"
            @select="select"
            @deselected="deselected"
          />
        </template>
        <template #action> <ActionViewVue @create="create" /></template>
      </ResumeView>
    </template>
    <template #movements>
      <MovementsView :movements="movementlist" @remove="remove" />
    </template>
  </LayoutView>
</template>
