<template>
  <v-container fill-height justify-center align-center>
    <v-flex xs12 sm6 md4 lg5>
      <v-card>
        <v-card-title mt-10> САСКЕ </v-card-title>

        <v-card-text mt-15>
          <v-form @submit.prevent="submit">
            <v-layout row wrap>
              <v-flex xs12 md4 mt-3>
                <v-subheader>Login</v-subheader>
              </v-flex>
              <v-flex xs12 md8>
                <v-text-field
                  class="input-group--focused mr-2"
                  name="login"
                  v-model="login"
                  label="login"
                  value="Input text"
                  :error-messages="loginErrors"
                  required
                  @input="$v.login.$touch()"
                  @blur="$v.login.$touch()"
                ></v-text-field>
              </v-flex>
            </v-layout>

            <v-btn class="mr-4" type="submit"> submit </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-container>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength, helpers } from "vuelidate/lib/validators";

const spaces = (value) => {
  if (typeof value === "undefined" || value === null || value === "") {
    return true;
  }
  return /^[a-zA-Z0-9-]*$/.test(value);
};

const languageRU = (value) => {
  if (typeof value === "undefined" || value === null || value === "") {
    return true;
  }
  return /^[a-zA-Z0-9- ]*$/.test(value);
};

export default {
  mixins: [validationMixin],

  validations: {
    login: { required, minLength: minLength(4), languageRU, spaces },
  },

  data: () => ({
    login: "",
  }),

  computed: {
    loginErrors() {
      const errors = [];
      if (!this.$v.login.$dirty) return errors;
      !this.$v.login.minLength &&
        errors.push("login must be at most 4 characters long");
      !this.$v.login.required && errors.push("login is required.");
      !this.$v.login.languageRU &&
        errors.push("Russian language is not allowed.");
      !this.$v.login.spaces && errors.push("Spaces is not allowed.");
      return errors;
    },
  },

  methods: {
    submit() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        // validation complite without mistakes
      } else {
        // validation mistakes found
      }
    },
  },
};
</script>