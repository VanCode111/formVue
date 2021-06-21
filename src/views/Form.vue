<template>
  <div class="container">
    <form action="" class="form" @submit.prevent="sendForm">
      <div class="form__contact">
        <h2 class="form__title">Контактные данные</h2>
        <div class="form__row">
          <inputText
            class="form__name form__item"
            v-model="name"
            type="text"
            title="Имя"
            :error="v$.name.$error"
            :isActive="name.length > 0"
            required="true"
          />
          <inputText
            class="form__surname form__item"
            v-model="surname"
            type="text"
            title="Фамилия"
            :error="v$.surname.$error"
            :isActive="surname.length > 0"
            required="true"
          />
          <inputText
            class="form__patronymyc form__item"
            v-model="patronymic"
            type="text"
            title="Отчество"
            :isActive="patronymic.length > 0"
            required="false"
          />
        </div>
        <div class="form__row">
          <inputText
            v-model="date"
            class="form__date form__item"
            type="date"
            title="Дата рождения"
            :isActive="date.length > 0"
            required="true"
          />
          <inputText
            type="text"
            class="form__phone form__item"
            :modelValue="phone"
            @update:modelValue="(val) => (phone = val)"
            title="Номер телефона"
            :isActive="phone.length > 1"
            required="true"
          />
          <Selector
            class="form__gender form__item"
            className="form__gender"
            title="Пол"
            name="gender"
            v-model="gender"
            :isActive="isActive(gender)"
            :options="[
              {
                text: 'М',
                value: 'male',
                id: 1,
              },
              {
                text: 'Ж',
                value: 'female',
                id: 2,
              },
            ]"
          />
        </div>
        <div class="form__row">
          <Selector
            class="form__clients form__item"
            name="doctor"
            className="form__clients"
            :multiple="true"
            v-model="clients"
            :required="true"
            :error="v$.clients.$error"
            title="Группа клиентов"
            :options="[
              {
                text: 'Иванов',
                value: 'Ivanov',
                id: 6,
              },
              {
                text: 'Захаров',
                value: 'Zakharov',
                id: 7,
              },
              {
                text: 'Чернышева',
                value: 'Chernisheva',
                id: 8,
              },
            ]"
          />
          <Selector
            class="form__doctor form__item"
            name="doctor"
            className="form__doctor"
            v-model="doctor"
            :isActive="isActive(doctor)"
            title="Лечащий врач"
            :options="[
              {
                text: 'Иванов',
                value: 'Ivanov',
                id: 3,
              },
              {
                text: 'Захаров',
                value: 'Zakharov',
                id: 4,
              },
              {
                text: 'Чернышева',
                value: 'Chernisheva',
                id: 5,
              },
            ]"
          />
        </div>
        <div class="form__row">
          <Checkbox
            id="checkbox-send"
            class="form__checkbox form__item"
            name="Отправить смс"
            v-model="sms"
          />
        </div>
      </div>
      <button class="form__button" type="submit">Добавить</button>
    </form>
  </div>
</template>

<script>
import Selector from "../components/Selector.vue";
import Checkbox from "../components/Checkbox.vue";
import inputText from "../components/textInput.vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
export default {
  name: "Form",

  data() {
    return {
      v$: useVuelidate(),
      date: "",
      name: "",
      surname: "",
      patronymic: "",
      gender: "",
      phone: "7",
      doctor: "",
      clients: [],
      sms: true,
    };
  },
  validations() {
    return {
      phone: { required },
      clients: { required },
      name: { required },
      surname: { required },
    };
  },
  watch: {
    phone() {
      setTimeout(() => {
        this.phone = "7" + this.phone.slice(1).replace(/[^0-9]/g, "");
      });
    },
  },
  methods: {
    isActive(value) {
      return value.length;
    },
    sendForm() {
      this.v$.$touch();
    },
  },
  components: {
    Selector,
    Checkbox,
    inputText,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap");

*,
*:after,
*:before {
  box-sizing: border-box;
}
body {
  font-family: "DM Sans", sans-serif;
  line-height: 1.5;
  padding: 0;
  min-width: 320px;
  background-color: #f0f8ff;
}

$main-color: #22b2ea;

.container {
  max-width: 1000px;
  margin: 0 auto;
}
input[type="text"] {
  text-rendering: none;
  appearance: none;
  color: transparent;
}

.form {
  padding: 20px;
  align-items: end;
  background-color: #fff;
  border-radius: 10px;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(12, 1fr);
  &__clients,
  &__doctor {
    width: 200px;
  }
  &__gender {
    width: 80px;
    flex-shrink: 0;
  }
  &__row + &__row {
    margin-top: 30px;
  }
  &__name,
  &__surname,
  &__patronymyc {
    max-width: 200px;
  }
  &__date {
    max-width: 180px;
    flex-shrink: 1;
  }
  &__phone {
    max-width: 200px;
    flex-shrink: 1.1;
  }
  &__item + &__item {
    margin-left: 15px;
  }
  &__row {
    display: flex;
    align-items: flex-end;
  }
  &__clients {
    grid-column: span 4;
  }

  &__doctor {
    grid-column: span 5;
  }
  &__checkbox {
    grid-column: span 3;
  }
  &__gender {
    max-width: 60px;
    z-index: 1000;
  }
  &__doctor {
    max-width: 200px;
  }
  &__input-block {
    position: relative;
    width: 100%;
    grid-column: span 4;
  }

  &__label {
    color: #8597a3;
    position: absolute;
    top: 0;
    left: 0px;
    transition: 0.25s ease;
    & span {
      color: red;
    }
  }
  &__button {
    background-color: $main-color;
    font: inherit;
    font-size: 1.25rem;
    padding: 10px;
    font-weight: 500;
    cursor: pointer;
    width: 100%;
    grid-column: span 3;
    grid-row: 3;
    font-weight: 500;
    border-radius: 6px;
    color: #fff;
    border: 0;
    &:focus {
      outline: 0;
    }
    &:hover {
      box-shadow: 0 0px 10px 0 rgba($main-color, 0.5);
    }
  }
}

@media (max-width: 430px) {
  .form {
    &__row {
      flex-direction: column;
    }
    &__item {
      max-width: none;
      width: 100%;
    }
    &__item + &__item {
      margin-top: 30px;
    }
    &__row + &__row {
      margin-top: 30px;
    }
  }
}
</style>
