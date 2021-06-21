<template>
  <div class="selector">
    <div
      :class="
        'selector__select ' +
          (multiple ? 'multi' : 'single') +
          ' ' +
          (modelValue.length > 0 ? 'active' : '') +
          ' ' +
          (error ? 'error' : '')
      "
    >
      <div class="selector__field" :onClick="() => (isOpen = !isOpen)">
        <span class="selector__title">{{ doctorName }}</span>
        <div :class="'selector__arrow ' + (isOpen ? 'down' : '')"></div>
      </div>
      <ul :class="'selector__options ' + (isOpen ? '' : 'hidden')">
        <li
          v-for="(option, index) in options"
          :key="index"
          class="selector__option-item"
        >
          <input
            type="radio"
            v-if="!multiple"
            :name="name"
            :id="option.id"
            class="selector__option"
            :checked="inputChecked(option.value)"
            @change="changeInput($event)"
            :value="option.value"
          />
          <Checkbox
            class="selector__checkbox"
            :id="option.id"
            :value="option.value"
            :modelValue="modelValue"
            @changes="(val) => $emit('update:modelValue', val)"
            :name="option.text"
            v-else
          />
          <label
            v-if="!multiple"
            :for="option.id"
            class="selector__label"
            v-on:click="selectOption(option.text)"
            >{{ option.text }}</label
          >
        </li>
      </ul>
    </div>
    <label for="" class="form__label"
      >{{ title }}<span>{{ required ? " *" : "" }}</span></label
    >
    <span v-if="error" class="error__title">Обязательное поле</span>
  </div>
</template>

<script>
import Checkbox from "../components/Checkbox.vue";
export default {
  components: { Checkbox },
  name: "Selector",
  data() {
    return {
      doctor: "",
      doctorName: "",
      isOpen: false,
    };
  },
  watch: {
    doctor() {
      console.log(this.doctor.value);
    },
  },
  methods: {
    closeList() {
      this.isOpen = false;
    },
    selectOption(doctorName) {
      this.closeList();
      this.doctorName = doctorName;
    },
    inputChecked(value) {
      return this.modelValue === value;
    },
    changeInput(e) {
      const value = e.target.value;
      this.$emit("update:modelValue", value);
    },
  },
  mounted() {
    console.log(this.modelValue);
    const selector = document.querySelector(".selector" + "." + this.className);
    document.addEventListener("click", (e) => {
      if (!e.path.includes(selector)) {
        this.closeList();
      }
    });
  },
  props: [
    "value",
    "isActive",
    "options",
    "title",
    "className",
    "multiple",
    "name",
    "modelValue",
    "required",
    "error",
  ],
};
</script>

<style scoped lang="scss">
$main-color: #22b2ea;

.selector {
  position: relative;
  color: #8597a3;
  &__option-item {
    width: 100%;
    box-sizing: content-box;
  }
  &__option-item + &__option-item {
    border-top: 1px solid #eee;
  }
  &__checkbox {
    width: 100%;
  }
  &__label {
    display: block;
    padding: 10px;
    cursor: pointer;
    box-sizing: content-box;
    width: 100%;
    z-index: 1;
    & span {
      color: #cd5c5c;
    }
  }
  &__options {
    border: 1px solid #eee;
    list-style: none;
    margin: 0px;
    padding: 0px;
    position: absolute;
    width: 100%;
    border-radius: 4px;
    background-color: white;
    box-sizing: content-box;
    z-index: 11;
    margin-top: 10px;
    overflow: hidden;
    //  border: 5px solid #eee;
    &.hidden {
      display: none;
    }
  }
  &__option {
    display: none;
    border &:hover {
      background-color: #8597a3;
    }
    &:checked + label {
      background-color: $main-color;
      color: white;
    }
  }
  &__field {
    height: 25px;
    cursor: pointer;
    position: relative;
    z-index: 10;
    color: black;
  }

  &__arrow {
    content: "";
    background: url("../assets/arrow-bottom.svg") center no-repeat;
    background-size: cover;
    width: 15px;
    height: 15px;
    opacity: 0.5;
    position: absolute;
    color: #8597a3;
    right: 2px;
    top: 50%;
    transform: translate(0, -50%);
    z-index: 10;
    transition: all 0.3s;
    &.down {
      transform: translate(0, -50%) rotate(180deg);
      transition: all 0.3s;
    }
  }
  &__select {
    appearance: none !important;
    outline: none;
    border: none;
    z-index: 12;
    background-color: transparent;
    border-bottom: 2px solid #eee;
    width: 100%;
    &.error {
      border-color: #ff4040;
    }
    &.multi,
    &.single {
      &.active {
        outline: 0;
        border-bottom: 2px solid $main-color;
        & + .form__label {
          color: $main-color;
        }
      }
    }
    &.single {
      &.active {
        & + .form__label {
          font-size: 14px;
          transform: translateY(-1rem);
          & span {
            color: red;
          }
        }
      }
    }
  }
}
.error__title {
  color: #ff4040;
  position: absolute;
  font-size: 12px;
  top: -1rem;
}
</style>
