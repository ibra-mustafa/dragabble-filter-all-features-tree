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
    <DataTable :value="data">
      <Column
        v-for="column in availableColumns"
        :key="column.field"
        :field="column.field"
        :header="column.header"
      ></Column>
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
const isDragging = ref(false);

const data = ref([
  { id: 1, name: "John", age: 25, city: "New York" },
  { id: 2, name: "Jane", age: 30, city: "London" },
  // Add more data rows as needed
]);

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
