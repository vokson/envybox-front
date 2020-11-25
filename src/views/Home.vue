<template>
  <div class="page">
    <div>
      <b-form @submit="onSubmit" @reset="onReset">
        <b-form-group
          id="input-group-name"
          label="Введите имя:"
          label-for="input-name"
        >
          <b-form-input
            id="input-name"
            v-model="form.name"
            placeholder="Ваше имя"
          ></b-form-input>
        </b-form-group>

        <div v-if="errors">
            <div v-for="(item, index) in errors.name" v-bind:key="index" class="alert alert-danger">{{ item }}</div>
        </div>

        <b-form-group
          id="input-group-phone"
          label="Введите номер телефона:"
          label-for="input-phone"
        >
          <b-form-input
            id="input-phone"
            v-model="form.phone"
            placeholder="10-ти значный номер без пробелов"
          ></b-form-input>
        </b-form-group>

        <div v-if="errors">
            <div v-for="(item, index) in errors.phone" v-bind:key="index" class="alert alert-danger">{{ item }}</div>
        </div>

        <b-form-group
          id="input-group-text"
          label="Обращение:"
          label-for="input-text"
        >
          <b-form-textarea
            id="input-text"
            v-model="form.text"
            placeholder="Введите что-нибудь..."
            rows="3"
            max-rows="6"
          ></b-form-textarea>
        </b-form-group>

        <div v-if="errors">
            <div v-for="(item, index) in errors.text" v-bind:key="index" class="alert alert-danger">{{ item }}</div>
        </div>

        <b-button type="submit" variant="primary" class="button"
          >Отправить</b-button
        >
        <b-button type="reset" variant="danger" class="button"
          >Сбросить</b-button
        >
      </b-form>
    </div>

    <div v-if="!on_progress" class="status_bar">
      <div v-if="errors">
          <div class="alert alert-danger">Ошибка выполнения запроса</div>
      </div>

      <div v-else>
        <div class="alert alert-success">Данные успешно отправлены.</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      url: "/feedback",
      form: {
        name: "",
        phone: "",
        text: "",
      },
      errors: null,
      on_progress: false,
      // status: null,
    };
  },
  methods: {
    onSubmit(evt) {
      this.on_progress = true;

      evt.preventDefault();

      let successFunction = function () {
        this.errors = null
      };

      let errorFunction = function (error) {
        if (error.response) {
          this.errors = error.response.data.errors;
        }
      };

      let finalFunction = function () {
        this.on_progress = false;
      };

      successFunction = successFunction.bind(this);
      errorFunction = errorFunction.bind(this);
      finalFunction = finalFunction.bind(this);

      window.$axios
        .post(this.url, this.form)
        .then(successFunction)
        .catch(errorFunction)
        .then(finalFunction);
    },

    onReset(evt) {
      evt.preventDefault();
      this.form.text = "";
      this.form.name = "";
      this.form.phone = "";
    },
  },
};
</script>

<style>
.page {
  margin: 1em;
}
.button {
  margin-left: 1em;
}
.status_bar {
  margin: 1em;
}
</style>
