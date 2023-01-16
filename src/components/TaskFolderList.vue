<template>
  <div class="task-folder-list">
    <div class="wrapper" v-if="folderList.length > 0">
      <div class="title-wrapper">
        <h1 class="title-folder" :style="{ color: selectedFolder?.color }">
          {{ selectedFolder?.title }}
        </h1>
      </div>

      <div class="no-tasks" v-if="!selectedFolder?.taskList.length">
        Нет задач
      </div>
      <transition-group name="list" tag="p">
        <div
          class="task-item-list"
          v-for="task in selectedFolder.taskList"
          :key="task.id"
        >
          <div class="task-item">
            <div class="pretty p-default p-round">
              <input
                type="checkbox"
                v-model="task.checked"
                @change="$emit('checked', task)"
              />
              <div class="state p-success-o task-text">
                <label :class="{ checkeds: task.checked }">{{
                  task.title
                }}</label>
              </div>
            </div>
            <span class="close" @click="$emit('removeTask', task)">x</span>
          </div>
        </div>
      </transition-group>

      <div
        class="add-task-item"
        v-show="!isOpenTaskPopap"
        @click="openOrCloseTaskPopap(true)"
      >
        <span>+</span>
        <span>Новая задача</span>
      </div>
      <transition name="fade"
        ><add-task-popap
          v-if="isOpenTaskPopap"
          :openOrCloseTaskPopap="openOrCloseTaskPopap"
          @newCreatedTask="taskTransfer"
      /></transition>
    </div>
    <div v-else class="no-tasks">Папки отсутствуют</div>
  </div>
</template>

<script lang="js">
import AddTaskPopap from './AddTaskPopap.vue';
export default {
components: {
  AddTaskPopap
},
props: {
  selectedFolder: Object,
  folderList: Array
},
data(){
  return {
    isOpenTaskPopap: false
  }
},
methods: {
  taskTransfer(taskItem){
   this.$emit('task', taskItem)
  },
  openOrCloseTaskPopap(bool){
    this.isOpenTaskPopap = bool
  }
}
}
</script>

<style lang="scss" scoped>
.task-folder-list {
  background: white;
  width: 100%;
  padding: 10px 2px;
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .title-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    column-gap: 15px;
    border-bottom: 1px solid #f2f2f2;
    h1 {
      font-family: "Montserrat";
      font-style: normal;
      font-weight: 700;
      font-size: 32px;
      line-height: 39px;
      color: #64c4ed;
      margin-bottom: 20px;
      text-transform: uppercase;
    }
    img {
      cursor: pointer;
      transition: 0.5s all;
      &:hover {
        transform: translateY(-4px);
      }
    }
  }
  .task-item-list {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    row-gap: 20px;
    .task-item {
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
      column-gap: 15px;

      input {
        cursor: pointer;
      }
      .task-text {
        font-family: "Lato";
        font-style: normal;
        font-weight: 600;
        font-size: 18px;
        line-height: 19px;
        color: #000000;
        margin-left: 15px;
      }
      .close {
        margin-left: 10px;
        color: #e3e3e3;
        cursor: pointer;
        transition: 0.5s all;
        &:hover {
          color: black;
        }
      }
    }
  }
  .no-tasks {
    font-family: "Montserrat";
    font-style: normal;
    font-weight: 700;
    font-size: 32px;
    line-height: 39px;
    color: #c9d1d3;
    text-align: center;
    margin-top: 40px;
  }
  .add-task-item {
    margin-top: 65px;
    cursor: pointer;
    transition: 0.5s all;
    &:hover {
      transform: translateY(-4px);
    }
    span {
      margin-left: 10px;
      text-align: center;
      color: #767676;
    }
  }

  .checkeds {
    text-decoration: line-through;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s ease;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }

  .list-item {
    display: inline-block;
    margin-right: 10px;
  }
  .list-enter-active,
  .list-leave-active {
    transition: all 0.3s ease;
  }
  .list-enter-from,
  .list-leave-to {
    opacity: 0;
    transform: translateX(30px);
  }
}
</style>
