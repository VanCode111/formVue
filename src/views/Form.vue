<template>
  <div class="container">
    <Notification
      :isActive="activeNotification"
      :srcImg="notification.srcImg"
      :text="notification.text"
    />
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
          />
        </div>
        <div class="form__row">
          <inputText
            v-model="date"
            class="form__date form__item"
            type="date"
            title="Дата рождения"
            :isActive="date.length > 0"
            :error="v$.date.$error"
            required="true"
          />
          <inputText
            type="text"
            class="form__phone form__item"
            :modelValue="phone"
            @update:modelValue="(val) => (phone = val)"
            title="Номер телефона"
            :isActive="phone.length > 10"
            :errorText="'11 символов'"
            required="true"
            :error="v$.phone.$error"
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
                text: 'VIP',
                value: 'VIP',
                id: 6,
              },
              {
                text: 'Проблемные',
                value: 'Проблемные',
                id: 7,
              },
              {
                text: 'ОМС',
                value: 'ОМС',
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
            name="Не отправлять СМС"
            v-model="sms"
          />
        </div>
      </div>
      <div class="form__address">
        <h2 class="form__title">Адрес</h2>
        <div class="form__row">
          <inputText
            class="form__country form__item"
            v-model="country"
            type="text"
            title="Страна"
            :isActive="country.length > 0"
          />
          <inputText
            class="form__oblast form__item"
            v-model="oblast"
            type="text"
            title="Область"
            :isActive="oblast.length > 0"
          />
          <inputText
            class="form__city form__item"
            v-model="city"
            type="text"
            title="Город"
            :error="v$.city.$error"
            :isActive="city.length > 0"
            :required="true"
          />
        </div>
        <div class="form__row">
          <inputText
            class="form__street form__item"
            v-model="street"
            type="text"
            title="Улица"
            :isActive="street.length > 0"
          />
          <inputText
            class="form__house form__item"
            v-model="house"
            type="text"
            title="Дом"
            :isActive="house.length > 0"
          />
        </div>
        <div class="form__row">
          <inputText
            class="form__index form__item"
            v-model="index"
            type="text"
            title="Индекс"
            :isActive="index.length > 0"
          />
        </div>
      </div>
      <div class="form__document">
        <h2 class="form__title">Документ</h2>
        <div class="form__row">
          <Selector
            class="form__typeOfDocument form__item"
            className="form__typeOfDocument"
            v-model="typeofdocument"
            :isActive="isActive(typeofdocument)"
            title="Тип документа"
            :required="true"
            :error="v$.typeofdocument.$error"
            :options="[
              {
                text: 'Паспорт',
                value: 'Паспорт',
                id: 9,
              },
              {
                text: 'Свидетельство о рождении',
                value: 'Свидетельство о рождении',
                id: 10,
              },
              {
                text: 'Вод. удостоверение',
                value: 'Вод. удостоверение',
                id: 11,
              },
            ]"
          />
        </div>
        <div class="form__row">
          <inputText
            type="text"
            class="form__seria form__item"
            v-model="seria"
            title="Серия"
            :isActive="seria.length > 0"
          />
          <inputText
            type="text"
            class="form__number form__item"
            v-model="number"
            title="Номер"
            :isActive="number.length > 0"
          />
        </div>
        <div class="form__row">
          <inputText
            type="text"
            class="form__whogive form__item"
            v-model="whogive"
            title="Кем выдан"
            :isActive="whogive.length > 0"
          />
        </div>
        <div class="form__row">
          <inputText
            v-model="dateofget"
            class="form__dateofget form__item"
            type="date"
            title="Дата выдачи"
            :required="true"
            :isActive="dateofget.length > 0"
            :error="v$.dateofget.$error"
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
import Notification from "../components/Notification.vue";
import useVuelidate from "@vuelidate/core";
import { required, minLength } from "@vuelidate/validators";
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
      index: "",
      country: "",
      city: "",
      house: "",
      street: "",
      oblast: "",
      typeofdocument: "",
      activeNotification: false,
      seria: "",
      number: "",
      whogive: "",
      dateofget: "",
      sms: true,
      notification: {
        isActive: false,
        text: "",
        srcImg: "",
      },
    };
  },
  validations() {
    return {
      phone: { minLength: minLength(11) },
      clients: { required },
      name: { required },
      surname: { required },
      city: { required },
      typeofdocument: { required },
      dateofget: { required },
      date: { required },
    };
  },
  watch: {
    activeNotification() {
      if (this.activeNotification) {
        setTimeout(() => {
          this.activeNotification = false;
        }, 3000);
      }
    },
    phone() {
      setTimeout(() => {
        this.phone =
          "7" +
          this.phone
            .slice(1)
            .replace(/[^0-9]/g, "")
            .slice(0, 10);
      });
    },
    index() {
      setTimeout(() => {
        this.index = this.index.replace(/[^0-9]/g, "");
      });
    },
  },
  methods: {
    isActive(value) {
      return value.length > 0;
    },
    showNotification(srcImg, text) {
      this.notification.srcImg = srcImg;
      this.notification.text = text;
      this.activeNotification = true;
    },
    sendForm() {
      this.v$.$touch();
      this.v$.$validate();
      if (!(this.v$.$errors.length > 0)) {
        this.showNotification(
          require("../assets/hand.png"),
          "Клиент успешно создан"
        );
      } else {
        this.showNotification(
          require("../assets/close.svg"),
          "Некорректные поля"
        );
      }
    },
  },
  components: {
    Selector,
    Checkbox,
    inputText,
    Notification,
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
  color: transparent;
}

.form {
  padding: 20px;
  align-items: end;
  background-color: #fff;
  border-radius: 10px;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(12, 1fr);
  &__typeOfDocument {
    width: 300px;
    flex-shrink: 0;
  }
  &__whogive {
    max-width: 600px;
  }
  &__dateofget,
  &__country,
  &__city,
  &__oblast,
  &__house,
  &__street,
  &__index,
  &__seria,
  &__number {
    max-width: 200px;
  }
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
    max-width: 200px;
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
    margin-top: 20px;
    width: 100%;
    grid-column: span 3;
    grid-row: 3;
    font-weight: 500;
    border-radius: 6px;
    color: #fff;
    max-width: 300px;
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
    &__button {
      width: 100%;
    }
    &__row {
      flex-direction: column;
    }
    &__item {
      max-width: none;
      width: 100%;
    }
    &__item + &__item {
      margin-left: 0;
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
