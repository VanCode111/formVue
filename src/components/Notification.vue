<template>
  <div :class="'notification ' + (isActive ? 'active' : '')">
    <h2 class="notification__text">{{ text }}</h2>
    <img :src="srcImg" alt="hand" class="notification__icon" />
  </div>
</template>

<script>
export default {
  props: ["text", "isActive", "srcImg"],
};
</script>

<style scoped lang="scss">
.notification {
  position: fixed;
  background-color: #fff;
  border-radius: 5px;
  top: -100%;
  left: 50%;
  width: 400px;
  z-index: 10000;
  margin-top: 10px;
  -webkit-box-shadow: 0px 0px 8px 0px rgba(34, 60, 80, 0.2);
  -moz-box-shadow: 0px 0px 8px 0px rgba(34, 60, 80, 0.2);
  box-shadow: 0px 0px 8px 0px rgba(34, 60, 80, 0.2);
  transform: translate(-50%, 0);
  transition: all 0.8s;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  &__icon {
    width: 50px;
    height: auto;
    margin-left: 10px;
  }
  &__text {
    color: #4d4d4d;
    margin: 0px;
  }
  &.active {
    top: 0;
    transition: all 0.8s;
  }
}
@media (max-width: 430px) {
  .notification {
    width: calc(100% - 20px);
    justify-content: space-between;
    &__text {
      font-size: 20px;
    }
    &__icon {
      width: 35px;
      height: auto;
    }
  }
}
</style>
