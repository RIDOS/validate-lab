<template>
  <form @submit.prevent="submitForm">
    <!--  -->
    <p>
      <label for="">Имя:</label
      ><input
        v-model.trim="name"
        type="text"
        :class="{
          invalid:
            (v$.name.$dirty && v$.name.required.$invalid) ||
            (v$.name.$dirty && v$.name.minLength.$invalid),
        }"
      />
    </p>
    <small v-if="v$.name.$dirty && v$.name.required.$invalid">
      Это поле обязательно для заполнения</small
    >
    <small v-else-if="v$.name.minLength.$invalid"
      >Имя должно содержать не менее 5-ти символов</small
    >
    <p>
      <label for="">Email:</label
      ><input
        type="text"
        v-model.trim="email"
        :class="{
          invalid:
            (v$.name.$dirty && v$.email.required.$invalid) ||
            v$.email.email.$invalid,
        }"
      />
    </p>
    <small v-if="v$.name.$dirty && v$.email.required.$invalid">
      Это поле обязательно для заполнения</small
    >
    <small v-else-if="v$.email.email.$invalid"
      >Введен не корректный email</small
    >
    <p>
      <label for="">Возраст:</label
      ><input
        type="text"
        v-model.trim="age"
        :class="{ invalid: v$.age.numeric.$invalid || v$.age.between.$invalid }"
      />
    </p>
    <small v-if="v$.age.numeric.$invalid">
      В этом поле может быть только число</small
    >
    <small v-else-if="v$.age.between.$invalid"
      >Введите значение от 18 до 70{{ v$.age.between.$message }}</small
    >
    <p></p>
    <button>Отправить</button>
  </form>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import {
  required,
  minLength,
  email,
  numeric,
  between,
} from "@vuelidate/validators";

export default {
  name: "App",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      name: "",
      email: "",
      age: "",
    };
  },
  validations() {
    return {
      name: { required, minLength: minLength(5) },
      email: { required, email },
      age: { numeric, between: between(18, 70) },
    };
  },
  methods: {
    submitForm() {
      this.v$.$touch();
      if (this.v$.$error) return;
    },
  },
};
</script>

<style>
</style>
