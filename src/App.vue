<template>
  <form @submit.prevent="submitForm">
    <h1>Анкета</h1>

    <p>Введите вашу почту:</p>
    <input
      type="text"
      v-model.trim="mailru"
      :class="{
        invalid:
          (v$.mailru.$dirty && v$.mailru.required.$invalid) ||
          v$.mailru.email.$invalid,
        valid: !v$.mailru.required.$invalid && !v$.mailru.email.$invalid,
      }"
      name="email"
    />
    <small v-if="v$.mailru.$dirty && v$.mailru.required.$invalid" class="error"
      >Это поле обязательно для заполнения!</small
    >

    <p>Введите ваше имя:</p>
    <input
      v-model.trim="name"
      :class="{
        invalid:
          (v$.name.$dirty && v$.name.required.$invalid) ||
          v$.name.minLength.$invalid,
        valid: !v$.name.required.$invalid && !v$.name.minLength.$invalid,
      }"
      type="text"
      name="name"
    />
    <small v-if="v$.name.$dirty && v$.name.required.$invalid" class="error"
      >Это поле обязательно для заполнения!</small
    >
    <small v-else-if="v$.name.minLength.$invalid" class="error"
      >Имя должно содержать не менее 4 символов!</small
    >

    <p>Какой ваш возраст?</p>
    <input
      type="number"
      v-model.trim="age"
      name="age"
      :class="{
        invalid: v$.age.numeric.$invalid || v$.age.between.$invalid,
        valid: v$.age.numeric.$invalid && v$.age.between.$invalid,
      }"
    />
    <small v-if="v$.age.numeric.$invalid" class="error">Только числа!</small>
    <small v-else-if="v$.age.between.$invalid" class="error">
      От 18 до 70 лет!
    </small>

    <p>Выберите ваш пол:</p>
    <input type="radio" v-model.trim="gender" name="gender" value="male" />
    Мужской<br />
    <input type="radio" v-model.trim="gender" name="gender" value="female" />
    Женский<br />
    <small v-if="v$.gender.$dirty && v$.gender.required.$invalid" class="error"
      >Пол обязательное поле!</small
    >

    <p>На каком вы курсе?</p>
    <input
      :class="{
        invalid: v$.age.numeric.$invalid || v$.age.between.$invalid,
      }"
      type="number"
      v-model.trim="study"
      name="study"
    />
    <small v-if="v$.study.numeric.$invalid" class="error">Только числа!</small>
    <small v-else-if="v$.study.between.$invalid" class="error">
      Только 1 и до 5 курса! (магистры идите лесом)
    </small>

    <p>Какое ваше любимое время года и почему?</p>
    <input
      :class="{
        invalid: v$.vin.$dirty && v$.vin.between.$invalid === false,
        valid: v$.vin.$dirty || v$.vin.between.$invalid === true,
      }"
      v-model.trim="vin"
      type="text"
      name="vin"
    />
    <small
      v-if="v$.vin.$dirty && v$.vin.between.$invalid === false"
      class="error"
      >Напишите подробнее...</small
    >

    <p>Какие книги вы любите читать?</p>
    <input
      :class="{
        invalid: v$.vin1.$dirty && v$.vin1.between.$invalid === false,
        valid: v$.vin1.$dirty || v$.vin1.between.$invalid === true,
      }"
      v-model.trim="vin1"
      type="text"
      name="vin1"
    />
    <small
      v-if="v$.vin1.$dirty && v$.vin1.between.$invalid === false"
      class="error"
      >Напишите подробнее...</small
    >

    <p>Что вам больше нравится, город или природа?</p>
    <select
      v-model="selectedOption"
      :class="{
        invalid: v$.selectedOption.$error,
      }"
      name="option"
    >
      <option value="">Выберите опцию</option>
      <option value="city">город</option>
      <option value="vilage">природа</option>
    </select>
    <small v-if="v$.selectedOption.$error" class="error"
      >Выберите опцию...</small
    >

    <p>Какой ваш любимый вид спорта?</p>
    <input
      :class="{
        invalid: v$.vin2.$dirty && v$.vin2.between.$invalid === false,
        valid: v$.vin2.$dirty && v$.vin2.between.$invalid === true,
      }"
      v-model.trim="vin2"
      name="vin"
      type="text"
    />
    <small
      v-if="v$.vin2.$dirty && v$.vin2.between.$invalid === false"
      class="error"
      >Напишите подробнее...</small
    >

    <p>Какую музыку вы предпочитаете?</p>
    <input
      :class="{
        invalid: v$.vin3.$dirty && v$.vin3.between.$invalid === false,
        valid: v$.vin3.$dirty && v$.vin3.between.$invalid === true,
      }"
      v-model.trim="vin3"
      name="vin"
      type="text"
    />
    <small
      v-if="v$.vin3.$dirty && v$.vin3.between.$invalid === false"
      class="error"
      >Напишите подробнее...</small
    >

    <p>Что для вас важнее: материальное благосостояние или духовное?</p>
    <select
      v-model="selectedOption1"
      :class="{
        invalid: v$.selectedOption1.$error,
      }"
      name="option"
    >
      <option value="">Выберите опцию</option>
      <option selected value="material">материальное</option>
      <option value="duh">духовное</option>
    </select>
    <small v-if="v$.selectedOption1.$error" class="error"
      >Выберите опцию...</small
    >

    <p>
      Как вы относитесь к путешествиям и куда бы вы хотели отправиться в
      следующий раз?
    </p>
    <input
      :class="{
        invalid: v$.vin5.$dirty && v$.vin5.between.$invalid === false,
        valid: v$.vin5.$dirty && v$.vin5.between.$invalid === true,
      }"
      v-model.trim="vin5" type="text" />
    <small
      v-if="v$.vin5.$dirty && v$.vin5.between.$invalid === false"
      class="error"
      >Напишите подробнее...</small
    >

    <p>Выберите ваш любимый цвет:</p>
    <input type="color" name="color" />

    <p>Какую еду вы предпочитаете?</p>
    <input v-model.trim="food" type="checkbox" name="food" value="pizza" />
    Пицца<br />
    <input v-model.trim="food1" type="checkbox" name="food" value="sushi" />
    Суши<br />
    <input v-model.trim="food2" type="checkbox" name="food" value="burgers" />
    Бургеры<br />
    <input v-model.trim="food3" type="checkbox" name="food" value="salad" />
    Салаты<br />
    <small
      v-if="
        v$.food.$dirty &&
        v$.food.required.$invalid &&
        v$.food1.required.$invalid &&
        v$.food2.required.$invalid &&
        v$.food3.required.$invalid
      "
      class="error"
      >Еда обязательное поле!</small
    >

    <p>Выберите ваш город:</p>
    <select
      v-model="selectedOption2"
      :class="{
        invalid: v$.selectedOption2.$error,
      }"
      name="option"
    >
      <option value="">Выберите опцию</option>
      <option value="ufa">Уфа</option>
      <option value="spb">Санкт-Петербург</option>
      <option value="ekb">Екатеринбург</option>
      <option value="kazan">Казань</option>
    </select>
    <small v-if="v$.selectedOption2.$error" class="error"
      >Выберите опцию...</small
    >

    <p>Какой ваш любимый фильм?</p>
    <textarea
      :class="{
        invalid: v$.vin.$dirty && v$.vin4.between.$invalid === false,
        valid: v$.vin4.$dirty && v$.vin4.between.$invalid === true,
      }"
      v-model.trim="vin4"
      name="vin"
    ></textarea>
    <small
      v-if="v$.vin.$dirty && v$.vin4.between.$invalid === false"
      class="error"
      >Напишите подробнее...</small
    >

    <br /><br />
    <input type="submit" value="Отправить" />
  </form>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import {
  between,
  required,
  email,
  minLength,
  numeric,
} from "@vuelidate/validators";

export default {
  name: "App",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      name: "",
      age: "",
      study: "",
      mailru: "",
      gender: "",
      vin: "",
      vin1: "",
      vin2: "",
      vin3: "",
      vin4: "",
      vin5: "",
      selectedOption: "",
      selectedOption1: "",
      selectedOption2: "",
      food: "",
      food1: "",
      food2: "",
      food3: "",
    };
  },
  validations() {
    return {
      name: { required, minLength: minLength(4) },
      mailru: { required, email },
      age: { numeric, between: between(18, 70) },
      gender: { required },
      food: { required },
      food1: { required },
      food2: { required },
      food3: { required },
      selectedOption: { required },
      selectedOption1: { required },
      selectedOption2: { required },
      vin: { between: between(4, 255) },
      vin1: { between: between(4, 255) },
      vin2: { between: between(4, 255) },
      vin3: { between: between(4, 255) },
      vin4: { between: between(4, 255) },
      vin5: { between: between(4, 255) },
      study: { numeric, between: between(1, 5) },
    };
  },
  methods: {
    submitForm() {
      console.log(this.v$.vin.between);
      this.v$.$touch();
      if (this.v$.$error) return;
    },
  },
};
</script>

<style></style>
