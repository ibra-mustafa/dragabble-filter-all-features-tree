<template>
  <div class="container">
    <div id="app">
      <button @click="dropIt">Options</button>
      <transition name="slide">
        <div class="list" v-if="isDropped">
          <draggable v-model="availableColumns" item-key="id">
            <template #item="{ element }">
              <div>{{ element.name }}</div>
            </template>
          </draggable>
        </div>
      </transition>
    </div>
    <DataTable :value="items">
      <Column field="name" header="Name"></Column>
      <Column field="age" header="Age"></Column>
      <Column field="city" header="City"></Column>
      <Column field="actions" header="Actions">
        <template #body="slotProps">
          <div class="mrgn">
            <span
              v-if="disable"
              class="pi pi-power-off pi-power-off-red"
              @click="callPropsDisableFunction(slotProps)"
            ></span>
            <span
              class="pi pi-trash"
              @click="callPropsDeleteFunction(slotProps)"
            ></span>
            <span
              v-if="edit"
              class="pi pi-pencil"
              @click="callPropsEditFunction(slotProps)"
            ></span>
            <span
              v-if="view"
              class="pi pi-eye"
              @click="callPropsViewFunction(slotProps)"
            ></span>
            <span
              v-if="imperson"
              class="pi pi-user"
              @click="callPropsImpersonateFunction(slotProps)"
            ></span>
          </div>
        </template>
      </Column>
    </DataTable>
  </div>
</template>

<script setup lang="ts">
const availableColumns = ref([
  { name: "name", field: "name", header: "Name" },
  { name: "age", field: "age", header: "Age" },
  { name: "city", field: "city", header: "City" },
  // Add more columns as needed
]);
const isDropped = ref();
const selectedColumns = ref(availableColumns.value);

interface Item {
  id: number;
  name: string;
  age: number;
  city: string;
  disabled: boolean;
}

const items = ref<Item[]>([
  { id: 1, name: "John", age: 25, city: "New York", disabled: false },
  { id: 2, name: "Jane", age: 30, city: "London", disabled: false },
  // Add more items as needed
]);

const deleteItem = (item: Item) => {
  const index = items.value.indexOf(item);
  if (index !== -1) {
    items.value.splice(index, 1);
  }
};

const editItem = (item: Item) => {
  // Handle editing logic here
};

const toggleDisabled = (item: Item) => {
  item.disabled = !item.disabled;
};

const dropIt = () => {
  isDropped.value = !isDropped.value;
};
</script>

<style>
.container {
  display: flex;
  align-items: center;
}

.dragging {
  border: 2px dashed #ccc;
}

.list {
  position: absolute;
  width: 204px;
  margin: 0;
  padding: 0;
  list-style-type: none;
  transform-origin: top;
  transition: transform 0.4s ease-in-out;
  overflow: hidden;
}
.list li {
  padding: 10px;
  background: white;
  border-bottom: solid thin #eee;
  border-left: solid medium #cbc;
}

.slide-enter,
.slide-leave-to {
  transform: scaleY(0);
}
</style>
