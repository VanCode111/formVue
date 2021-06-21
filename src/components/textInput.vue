<template>
  <div class="form__input-block">
    <input
      :type="type"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      :class="
        'form__input ' +
          (isActive ? 'active' : '') +
          ' ' +
          (error ? 'error' : '')
      "
    />
    <label for="" class="form__label"
      >{{ title }} <span>{{ required ? "*" : "" }}</span></label
    >
    <span v-if="error" class="error__title">{{
      errorText ? "Номер из 11 цифр" : "Обязательное поле"
    }}</span>
  </div>
</template>

<script>
export default {
  name: "textInput",
  props: [
    "modelValue",
    "isActive",
    "title",
    "required",
    "error",
    "type",
    "errorText",
  ],
  watch: {
    modelValue() {},
  },
};
</script>

<style scoped lang="scss">
$main-color: #22b2ea;

input[type="date"] {
  font-family: "DM Sans", sans-serif;
  &::-webkit-datetime-edit {
    color: transparent;
  }
  &::-webkit-calendar-picker-indicator {
    opacity: 0.4;
    cursor: pointer;
  }
}

input[type="date"]:focus::-webkit-datetime-edit,
input[type="date"].active::-webkit-datetime-edit {
  color: #8597a3;
}

input {
  -webkit-appearance: none;
  -webkit-border-radius: 0;
  &.error {
    border-color: red;
  }
  border: 0px;
  position: relative;
  z-index: 10;
  background-color: transparent;
  border-bottom: 2px solid #eee;
  padding: 5px 0;
  display: block;
  width: 100%;
  &:focus,
  &.active {
    outline: 0;
    color: black;
    border-bottom: 2px solid $main-color;
    & + .form__label {
      & + .error__title {
        display: none;
      }
      color: $main-color;
      font-size: 14px;
      transform: translateY(-1rem);
    }
  }
}
input[type="date"] {
  height: 25px;
}
.form__label {
  color: #8597a3;
  position: absolute;
  top: 0;
  left: 0px;
  transition: 0.25s ease;
  & span {
    color: red;
  }
}
.error__title {
  color: #ff4040;
  position: absolute;
  font-size: 12px;
  top: -1rem;
}
</style>
