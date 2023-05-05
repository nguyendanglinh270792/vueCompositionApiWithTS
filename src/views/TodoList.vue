<template>
  <section
    class="w-screen h-screen flex justify-center items-center bg-black/80">
    <div
      class="w-[500px] h-[600px] bg-slate-500 border rounded-xl font-mono text-white">
      <span
        class="flex justify-center items-center mt-10 gap-x-4 h-[30px] w-full">
        <p class="text-[20px ]">Add List:</p>
        <input
          ref="refTaskNameInput"
          v-model="taskName"
          type="text"
          class="p-2 rounded-md text-black"
          required />
        <button
          @click="handlerAddTask"
          type="button"
          class="text-white bg-gradient-to-r from-green-400 via-green-500 to-green-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-green-300 dark:focus:ring-green-800 shadow-lg shadow-green-500/50 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2">
          AddNew
        </button>
      </span>
      <span
        class="flex justify-center flex-col ml-5 mr-5 gap-y-4 mt-10 text-black">
        <ul>
          <li
            class="p-3 border flex justify-between"
            v-for="(task, index) in taskList"
            :key="index">
            <input
              :value="task"
              :disabled="isEditable"
              @input="handlerChangValue" />
            <span class="flex gap-x-4">
              <img
                @click="handlerDeleteTask(index)"
                src="../imgs/delete.png"
                alt="delete-icon"
                class="w-4 h-4 cursor-pointer" />
              <img
                @click="handlerUpdateTask"
                src="../imgs/edit.png"
                alt="edit-icon"
                class="w-4 h-4 cursor-pointer" />
              <img
                @click="handlerSaveTask(index)"
                src="../imgs/save.png"
                alt="save-icon"
                class="w-4 h-4 cursor-pointer" />
            </span>
          </li>
        </ul>
      </span>
    </div>
  </section>
</template>
<script lang="ts">
import { defineComponent, reactive, ref, toRefs } from "vue";
export default defineComponent({
  setup() {
    const state = reactive({
      taskName: "" as string,
      taskUpdate: "" as string,
      taskList: [] as string[],
      isEditable: true as boolean,
    });
    const refTaskNameInput = ref<HTMLInputElement | null>(null);
    const handlerAddTask = () => {
      if (state.taskName !== "") {
        state.taskList = [...state.taskList, state.taskName];
        state.taskName = "";
        if (refTaskNameInput.value !== null) {
          refTaskNameInput.value.focus();
        }
        console.log(state.taskList);
      } else {
        alert("Task is empty.");
      }
      console.log(state.taskList);
    };
    const handlerDeleteTask = (index: number) => {
      state.taskList.splice(index, 1);
    };
    const handlerUpdateTask = () => {
      state.isEditable = false;
    };
    const handlerSaveTask = (index: number) => {
      state.taskList[index] = state.taskUpdate;

      state.isEditable = true;
    };
    const handlerChangValue = (e: Event) => {
      state.taskUpdate = (e.target as HTMLInputElement).value;
    };
    return {
      ...toRefs(state),
      handlerAddTask,
      handlerDeleteTask,
      handlerUpdateTask,
      handlerSaveTask,
      handlerChangValue,
    };
  },
});
</script>
