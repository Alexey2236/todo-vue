<template>
  <div class="app">
    <transition name="fade"
      ><side-bar
        v-if="openSideBar"
        :folderList="folderList"
        :openOrClosePopap="openOrClosePopap"
        :isOpen="isOpen"
        :selectedFolder="selectedFolder"
        @remove="removeFolder"
        @selectFolder="selectFolderFn"
        @createFolder="addFolder"
    /></transition>

    <div class="wrapper">
      <div class="b" @click="toogleOpenSidebar">
        {{ openSideBar ? "◀" : "▶" }}
      </div>

      <task-folder-list
        :selectedFolder="selectedFolder"
        :folderList="folderList"
        @task="addTask"
        @removeTask="removeTasks"
        @checked="toogleChecked"
      />
    </div>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import TaskFolderList from "./components/TaskFolderList.vue";

export default {
  name: "App",
  components: {
    SideBar,
    TaskFolderList,
  },
  data() {
    return {
      folderList: [],
      isOpen: false,
      selectedFolder: null,
      openSideBar: false,
    };
  },

  mounted() {
    if (localStorage.getItem("folderList")) {
      try {
        this.folderList = JSON.parse(localStorage.getItem("folderList")) || [];
      } catch (e) {
        localStorage.removeItem("folderList");
      }
    }

    if (this.folderList.length) {
      this.selectedFolder = this.folderList[0];
    }
  },

  methods: {
    toogleOpenSidebar() {
      this.openSideBar = !this.openSideBar;
    },
    saveToLocalStorage(obj, name) {
      const parsed = JSON.stringify(obj);
      localStorage.setItem(name, parsed);
    },
    toogleChecked(task) {
      this.folderList.forEach((item) => {
        if (item.id === this.selectedFolder.id) {
          item.taskList.checked = task.checked;
        }
      });

      this.saveToLocalStorage(this.folderList, "folderList");
    },
    addTask(taskItem) {
      if (taskItem.title === "") {
        return;
      }
      this.folderList.forEach((item) => {
        if (item.id === this.selectedFolder.id) {
          item.taskList.unshift(taskItem);
          this.saveToLocalStorage(this.folderList, "folderList");
        }
      });
    },
    removeTasks(task) {
      this.folderList.forEach((item) => {
        if (item.id === this.selectedFolder.id) {
          item.taskList = item.taskList.filter((item) => item.id !== task.id);
          this.saveToLocalStorage(this.folderList, "folderList");
        }
      });
    },
    removeFolder(folder) {
      this.folderList = this.folderList.filter((item) => item.id !== folder.id);
      this.saveToLocalStorage(this.folderList, "folderList");
      if (this.folderList.length > 0 && folder.id === this.selectedFolder.id) {
        this.selectedFolder = this.folderList[0];
      }
    },
    addFolder(newFolder) {
      if (newFolder.title === "") {
        return;
      }
      this.folderList.unshift(newFolder);
      this.isOpen = false;
      this.saveToLocalStorage(this.folderList, "folderList");
      this.selectedFolder = this.folderList[0];
      this.openSideBar = false;
    },
    openOrClosePopap(bool) {
      this.isOpen = bool;
    },
    selectFolderFn(folder) {
      if (folder === undefined) {
        folder = this.selectedFolder;
      }
      this.folderList.forEach((item) => {
        if (item.id === folder.id) {
          this.selectedFolder = folder;
        }
      });
      this.openSideBar = false;
    },
  },
};
</script>

<style lang="scss">
body {
  background: #e5e5e5;
  font-family: "Lato", sans-serif;
  font-family: "Montserrat", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 0px 10px;
}

.app {
  margin: 10px auto 0 auto;
  max-width: 700px;
  position: relative;
  box-shadow: 4px 2px 52px 0px rgba(34, 60, 80, 0.2);
  border-radius: 10px;
}
.wrapper {
  display: flex;
  min-height: 600px;
}
.b {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 25px;
  background-color: #dbf0f8;
  cursor: pointer;
}

* {
  box-sizing: border-box;
}

ul,
li,
dl {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 0;
}

a {
  text-decoration: none;
  color: inherit;
  display: inline-block;
  &:hover {
    text-decoration: none;
    color: inherit;
  }
}

p {
  margin: 0;
  padding: 0;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input:focus {
  outline: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
