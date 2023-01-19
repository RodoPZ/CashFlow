<script setup lang="ts">
import { toRefs, defineEmits } from "vue";
import MovementItem from "./MovementItem.vue";
import type { MovementTypes } from "@/components/models/Movement.model";

const props = defineProps({
  movements: {
    type: Array<MovementTypes>,
    default: () => [],
  },
});

const { movements } = toRefs(props);
const emit = defineEmits(["remove"]);
const remove = (id: Number) => {
  emit("remove", id);
};
</script>

<template>
  <div class="movements">
    <h2 class="title">Historial</h2>
    <div class="content" v-if="movements">
      <MovementItem
        v-for="(movement, index) in movements"
        :key="index"
        :id="movement.id"
        :description="movement.description"
        :amount="movement.amount"
        :title="movement.title"
        @remove="remove"
      >
      </MovementItem>
    </div>
  </div>
</template>

<style scoped>
.movements {
  max-height: 100%;
  padding: 0 8px;
  margin-bottom: 14px;
}
.title {
  margin: 8px 16px 24px 16px;
  color: var(--brand-blue);
}
.content {
  max-height: 68vh;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow-y: scroll;
}
</style>
