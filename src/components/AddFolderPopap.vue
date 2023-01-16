<template>
  <div class="add-folder-popap">
    <div class="close-popap" @click="openOrClosePopap(false)">×</div>

    <my-input
      placeholder="Название папки"
      v-model.trim="newFolder.title"
      :modelValue="$event"
    />
    <div class="select-color">
      <div
        v-for="circle in circleList"
        :key="circle.color"
        class="color-item"
        :class="{ active: circle.active }"
        :style="{
          background: circle.color,
        }"
        @click="addColor(circle.color)"
      ></div>
    </div>
    <my-button class="add" @click="createFolders">Добавить</my-button>
  </div>
</template>

<script>
import MyButton from "./UI/MyButton.vue";
import MyInput from "./UI/MyInput.vue";

export default {
  components: { MyButton, MyInput },
  props: {
    openOrClosePopap: Function,
  },
  data() {
    return {
      circleList: [
        {
          color: "#c9d1d3",
          active: false,
        },
        {
          color: "#42b883",
          active: false,
        },
        {
          color: "#64c4ed",
          active: false,
        },
        {
          color: "#ffbbcc",
          active: false,
        },
        {
          color: "#b6e6bd",
          active: false,
        },
        {
          color: "#c355f5",
          active: false,
        },
        {
          color: "#09011a",
          active: false,
        },
        {
          color: "#ff6464",
          active: false,
        },
      ],
      selectColor: "",
      newFolder: {
        id: 0,
        title: "",
        color: "ff6464",
      },
    };
  },
  methods: {
    addColor(color) {
      this.circleList.forEach((circle) => (circle.active = false));
      this.circleList.forEach((circle) => {
        if (circle.color === color) {
          circle.active = true;
        }
      });
      this.selectColor = color;
    },
    createFolders() {
      if (this.selectColor === "") {
        return;
      }
      this.newFolder.color = this.selectColor;
      this.newFolder.id = Date.now();
      (this.newFolder.taskList = []),
        this.$emit("createFolder", this.newFolder);
      this.newFolder = {
        title: "",
      };
    },
  },
};
</script>

<style scoped lang="scss">
.add-folder-popap {
  margin-top: 20px;
  display: inline-block;
  width: 240px;
  position: relative;
  background: #ffffff;
  box-shadow: 0px 6px 10px rgba(0, 0, 0, 0.05);
  border-radius: 10px;
  border: solid 4px #4dd599;
  padding: 10px;
  .close-popap {
    background-color: #5e5b5b;
    color: #ffffff;
    width: 25px;
    height: 25px;
    border-radius: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    left: 95%;
    top: -10px;
    cursor: pointer;
  }

  .select-color {
    margin-top: 15px;
    display: flex;
    justify-content: space-between;

    .color-item {
      width: 25px;
      height: 25px;
      background: #42b883;
      border-radius: 100%;
      cursor: pointer;
      transition: 0.3s all;
      &:hover {
        transform: translateY(-3px);
      }
    }
  }
  .add {
    margin-top: 16px;
  }
}
.active {
  border: 2px solid #201f1f;
}
</style>
