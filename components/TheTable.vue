<template>
  <div class="container">
    <div id="app">
      <button @click="dropIt">Options</button>
      <transition name="slide">
        <div class="list" v-if="isDropped">
          <draggable v-model="availableColumns" item-key="id">
            <template #item="{ element }">
              <div  draggable="true">{{ element.name }}</div>
            </template>
          </draggable>
        </div>
      </transition>
    </div>
    <DataTable
    v-model:editingRows="editingRows"
        :value="data"
        editMode="row"
        dataKey="id"
        @row-edit-save="onRowEditSave"
        tableClass="editable-cells-table"
        tableStyle="min-width: 50rem">
      <Column
        v-for="column in availableColumns"
        :key="column.field"
        :field="column.field"
        :header="column.header" filterField="column.name" filterMatchMode="contains" sortable
      ><template  #editor="{ data, field }">
          <InputText v-model="data[field]" />
        </template></Column>
      <Column :rowEditor="true" style="width: 10%; min-width: 8rem" bodyStyle="text-align:center"></Column>

    </DataTable>
  </div>
</template>

<script setup lang="ts">

const props = defineProps({
  view: {
    required: false,
    default: false
  },
  edit: {
    required: false,
    default: true
  },
  disable: {
    required: false,
    default: false
  },
  imperson: {
    required: false,
    default: false
  },
  handleView: {
    type: Function,
    required: false
  },
  handleEdit: {
    type: Function,
    required: false
  },
  handleDelete: {
    type: Function,
    required: false
  },
  handleDisable: {
    type: Function,
    required: false
  },
  handleImpersonate: {
    type: Function,
    required: false
  }
})
const availableColumns = ref([
  { name: "name", field: "name", header: "Name" },
  { name: "age", field: "age", header: "Age" },
  { name: "city", field: "city", header: "City" },
  // Add more columns as needed
]);
const isDropped = ref();
const selectedColumns = ref(availableColumns.value);
const isDragging = ref(false);
const editingRows = ref([]);

const onRowEditSave = (event) => {
  let { newData, index } = event;

  data.value[index] = newData;
};
const data = ref([
  { id: 1, name: "John", age: 25, city: "New York" },
  { id: 2, name: "Jane", age: 30, city: "London" },
  // Add more data rows as needed
]);

const dropIt = () => {
  isDropped.value = !isDropped.value;
};
// const callPropsEditFunction = (info) => {
//   props.handleEdit?(info.data)
// }
// const callPropsDeleteFunction = (info) => {
//   props.handleDelete?(info.data)
// }
// const callPropsViewFunction = (info) => {
//   props.handleView?(info.data)
// }
// const callPropsDisableFunction = (info) => {
//   //props.handleDisable(info.data);
//   if (info.data.disabled) {
//     info.data.disabled = false
//   } else {
//     info.data.disabled = true
//   }
// }
// const callPropsImpersonateFunction = (info) => {
//   props.handleImpersonate?(info.data)
// }
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
  cursor: pointer;

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
