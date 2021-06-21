<template>
  <div class="checkbox">
    <input
      type="checkbox"
      :value="value"
      :checked="isChecked"
      @change="updateInput"
      :id="id"
      class="checkbox__input"
    />
    <label :for="id" class="checkbox__label">{{ name }}</label>
  </div>
</template>

<script>
export default {
  name: "Checkbox",
  data() {
    return {
      doctor: [],
    };
  },

  props: ["id", "name", "value", "modelValue"],
  computed: {
    isChecked() {
      if (this.modelValue instanceof Array) {
        return this.modelValue.includes(this.value);
      }
      return this.modelValue === true;
    },
  },
  methods: {
    updateInput(e) {
      const isChecked = e.target.checked;
      if (this.modelValue instanceof Array) {
        let newArr = [...this.modelValue];
        if (isChecked) {
          newArr.push(this.value);
        } else {
          newArr.splice(newArr.indexOf(this.value), 1);
        }
        this.$emit("changes", newArr);
      } else {
        this.$emit("update:modelValue", isChecked ? true : false);
      }
    },
  },
};
</script>

<style scoped lang="scss">
$main-color: #22b2ea;
.checkbox {
  cursor: pointer;
  &__label {
    display: flex;
    padding: 10px;
    cursor: pointer;
    color: #8597a3;
    &::before {
      content: "";
      display: inline-block;
      border: 2px solid #eee;
      border-radius: 4px;
      width: 20px;
      height: 20px;
      margin-right: 5px;
      display: block;
      padding: 5px;
    }
  }
  &__input {
    cursor: pointer;
    display: none;
    &:checked + label::before {
      background: url("../assets/check.svg") no-repeat center;
      background-size: 13px;
      border-color: $main-color;
    }
  }
}
</style>
