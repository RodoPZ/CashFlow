<script setup lang="ts">
import { ref, defineEmits } from "vue";
import ModalView from "./ModalView.vue";

const showModal = ref(false);
const title = ref("");
const amount = ref(0);
const description = ref("");
const movementsType = ref("Ingreso");

const emit = defineEmits(["create"]);

const submit = () => {
  showModal.value = !showModal.value;
  emit("create", {
    id: new Date().getTime(),
    title: title.value,
    decription: description.value,
    amount: movementsType.value === "Ingreso" ? amount.value : -amount.value,
    time: new Date(),
  });
  title.value = "";
  description.value = "";
  amount.value = 0;
  movementsType.value = "Ingreso";
};
</script>

<template>
  <button @click="showModal = true">Agregar Movimiento</button>
  <Teleport to="#app">
    <ModalView v-show="showModal" @close="showModal = false">
      <form @submit.prevent="submit">
        <div class="field">
          <label for="title">Título</label>
          <input type="text" v-model="title" id="title" />
        </div>
        <div class="field">
          <label for="amount">Monto</label>
          <input type="number" v-model="amount" id="amount" />
        </div>
        <div class="field">
          <label for="description">Descripción</label>
          <textarea rows="4" v-model="description" id="description"></textarea>
        </div>
        <div class="field">
          <label class="radio-label" for="">
            <input type="radio" v-model="movementsType" value="Ingreso" />
            <span>Ingreso</span>
          </label>
          <label class="radio-label" for="">
            <input type="radio" v-model="movementsType" value="Gasto" />
            <span>Gasto</span>
          </label>
        </div>
        <div class="action">
          <button>Agregar movimiento</button>
        </div>
      </form>
    </ModalView>
  </Teleport>
</template>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}

form {
  font-size: 1.24rem;
  width: 100%;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>
