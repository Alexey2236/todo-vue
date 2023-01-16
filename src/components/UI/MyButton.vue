<template>
  <button class="add"><slot></slot></button>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
.add {
  display: block;
  padding: 5px 30px;
  margin: 0px auto 0 auto;
  background: #4dd599;
  border-radius: 4px;
  border: none;
  font-family: "Lato";
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  line-height: 17px;
  text-align: center;
  letter-spacing: 0.15px;
  color: #ffffff;
  cursor: pointer;
  box-shadow: 3px 3px 6px 0px rgba(34, 60, 80, 0.6);
  transition: 0.5s all;
  &:active {
    box-shadow: 1px 1px 6px 0px rgba(34, 60, 80, 0.5);
  }
  &:hover {
    background-color: pink;
  }
}
</style>
