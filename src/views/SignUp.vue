<template>
  <div class="root">
    <h2>Create an Account</h2>
    <p>
      <input type="text" placeholder="username" v-model="state.username" />
      <span v-if="v$.username.$error">
        {{ v$.username.$errors[0].$message }}
      </span>
    </p>
    <p>
      <input type="password" placeholder="Password" v-model="state.password" />
      <span v-if="v$.password.$error">
        {{ v$.password.$errors[0].$message }}
      </span>
    </p>
    <button @click="submitForm">Submit</button>
  </div>
</template>

<script>
import { reactive, computed } from "vue";
import useValidate from "@vuelidate/core";
import { required, email, minLength } from "@vuelidate/validators";
import axios from "axios";

export default {
  name: "SignUp",
  components: {},
  mounted() {
    //API Call
  },
  setup() {
    const state = reactive({
      username: "",
      password: "",
    });

    const rules = computed(() => {
      return {
        username: {
          required,
          email,
        },
        password: { required, minLength: minLength(6) },
      };
    });
    const v$ = useValidate(rules, state);
    return { state, v$ };
  },
  data() {
    return {
      username: "",
      password: "",
    };
  },
  validations() {
    return {
      username: { required },
      password: { required },
    };
  },
  methods: {
    submitForm() {
      this.v$.$validate();
      if (!this.v$.$error) {
        axios
          .post(`http://127.0.0.1:8000/register/`, this.state, {
            headers: {},
          })
          .then((res) => {
            this.$router.push("/login");
          })
          .catch((err) => {
            if (err.response.data.username) {
              alert(err.response.data.username[0]);
            }
          });
      } else {
        alert("Form failed validation");
      }
    },

    goTo() {
      this.$router.back();
    },
  },
};
</script>

<style lang="css">
.root {
  width: 400px;
  margin: 0 auto;
  background-color: #fff;
  padding: 30px;
  margin-top: 100px;
  border-radius: 20px;
}
input {
  border: none;
  outline: none;
  border-bottom: 1px solid #ddd;
  font-size: 1em;
  padding: 5px 0;
  margin: 10px 0 5px 0;
  width: 100%;
}
button {
  background-color: #3498db;
  padding: 10px 20px;
  margin-top: 10px;
  border: none;
  color: white;
}
</style>
