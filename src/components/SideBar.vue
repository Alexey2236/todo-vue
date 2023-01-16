<template>
  <div class="side-bar">
    <div class="all-task-wrapper" v-if="folderList?.length > 0">
      <img src="@/assets/all-task.png" alt="" />
      <span class="all-task">Все задачи</span>
    </div>
    <div class="all-folder">
      <transition-group name="folder-list">
        <div
          class="task-folder"
          v-for="folder in folderList"
          :class="{ active: folder.id === selectedFolder?.id }"
          :key="folder.id"
          @click="$emit('selectFolder', folder)"
        >
          <div class="circle" :style="{ background: folder.color }"></div>
          <span>{{ folder.title }}</span>
          <span class="close" @click="$emit('remove', folder)">×</span>
        </div>
      </transition-group>
    </div>
    <div
      class="add-folder"
      v-if="isOpen !== true"
      @click="openOrClosePopap(true)"
    >
      <span>+</span>
      <span>Добавить папку</span>
    </div>

    <transition name="fade">
      <add-folder-popap
        v-if="isOpen"
        :openOrClosePopap="openOrClosePopap"
        @createFolder="folderTransfer"
    /></transition>
  </div>
</template>

<script lang="js">
import AddFolderPopap from './AddFolderPopap.vue';
export default {
components: {
  AddFolderPopap
},
props: {
  folderList: {
    type: Array,
      default: Array,
      required: true,
  },
  isOpen: Boolean,
  openOrClosePopap: Function,
  selectedFolder: Object
},
methods: {
  folderTransfer(newFolder){
   this.$emit('createFolder', newFolder)
  },
}



}
</script>

<style lang="scss">
.side-bar {
  position: absolute;
  z-index: 100;
  left: 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
  column-gap: 20px;
  min-width: 250px;
  height: 100%;
  padding: 50px 10px;
  background-color: rgba(239, 239, 240, 1);

  .all-folder {
    display: flex;
    flex-direction: column;
    row-gap: 20px;
    .task-folder {
      display: flex;
      align-items: center;
      justify-content: space-between;
      column-gap: 3px;
      cursor: pointer;
      transition: 0.5s all;
      &:hover {
        transform: translateY(-4px);
      }

      .circle {
        width: 15px;
        height: 15px;
        background: #ffbbcc;
        border-radius: 100%;
        margin-left: 10px;
      }
      span {
        font-family: "Lato";
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 16px;
        letter-spacing: 0.15px;
        color: #000000;
        text-transform: uppercase;
      }

      .close {
        color: #1a1919;
        margin-left: 30px;
        font-size: 20px;
      }
    }
  }
  .all-task-wrapper {
    display: flex;
    align-items: center;
    column-gap: 10px;
    margin-bottom: 40px;
    margin-right: 30px;
    transition: 0.5s all;

    .all-task {
      font-family: "Lato";
      font-style: normal;
      font-weight: 500;
      font-size: 20px;
      line-height: 19px;
      color: #000000;
    }
  }
  .add-folder {
    cursor: pointer;
    display: flex;
    align-items: center;
    column-gap: 10px;
    margin-top: 50px;
    margin-right: 20px;
    transition: 0.5s all;
    &:hover {
      transform: translateY(-4px);
    }

    span {
      font-family: "Lato";
      font-style: normal;
      font-weight: 600;
      font-size: 15px;
      line-height: 17px;
      letter-spacing: 0.15px;
      color: #767676;
    }
  }
}

.active {
  width: 105%;
  background: #ffffff;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
  padding: 10px 10px 10px 0;
}

.folder-list-item {
  display: inline-block;
  margin-right: 10px;
}
.folder-list-enter-active,
.folder-list-leave-active {
  transition: all 1s ease;
}
.folder-list-enter-from,
.folder-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
