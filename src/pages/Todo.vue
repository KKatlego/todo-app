<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" seperator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div v-if="tasks.length==0" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"></q-icon>
<div class="text-h5 text-primary text-center">
  No tasks
</div>
    </div>
  </q-page>
</template>

<script setup>
import { reactive, ref } from "vue";
import { useQuasar } from "quasar";
const $q = useQuasar();

let newTask = ref();
const tasks = reactive([
  // { title: "Create website", done: false },
  // { title: "Publish website", done: false },
  // { title: "Get advertising", done: false },
]);
const addTask = () => {
  tasks.push({
    title: newTask.value,
    done: false,
  });
  newTask.value = "";
};
const deleteTask = (index) => {
  $q.dialog({
    title: "Confirm",
    message: "Are you sure you want to delete?",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.splice(index, 1);
    $q.notify("Task deleted");
  });
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: bbb;
  }
}
.no-tasks{
  opacity: 0.5;
}
</style>
