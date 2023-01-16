<template>
  <div class="add-task">
    <my-input
      placeholder="Текст задачи"
      v-model.trim="taskItem.title"
      :modelValue="$event"
    />
    <div class="btns">
      <my-button class="add-task-btn" @click="createTasks">Добавить</my-button>
      <button class="canc" @click="openOrCloseTaskPopap(false)">Отмена</button>
    </div>
  </div>
</template>

<script>
import MyInput from "./UI/MyInput.vue";
import MyButton from "./UI/MyButton.vue";

export default {
  components: { MyInput, MyButton },
  props: {
    openOrCloseTaskPopap: Function,
  },
  data() {
    return {
      taskItem: {
        title: "",
        checked: false,
        id: 0,
      },
    };
  },
  methods: {
    createTasks() {
      this.taskItem.id = Date.now();
      this.$emit("newCreatedTask", this.taskItem);
      this.taskItem = {
        title: "",
      };
    },
  },
};
</script>

<style scoped lang="scss">
.add-task {
  width: 60%;
  margin: 15px auto 0 auto;

  .btns {
    margin-top: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    column-gap: 15px;

    .add-task-btn {
      padding: 5px 10px;
      margin: 0;
    }
    .canc {
      background: #f4f6f8;
      border-radius: 4px;
      padding: 5px 7px;
      border: none;
      font-family: "Lato";
      font-style: normal;
      font-weight: 600;
      font-size: 14px;
      line-height: 17px;
      text-align: center;
      letter-spacing: 0.15px;
      color: #9c9c9c;
      box-shadow: 2px 2px 6px 0px rgba(34, 60, 80, 0.5);
      transition: 0.5s all;

      &:active {
        box-shadow: 0px 0px 6px 0px rgba(34, 60, 80, 0.5);
      }
      &:hover {
        background-color: #4dd599;
        color: #f4f6f8;
      }
    }
  }
}
</style>
