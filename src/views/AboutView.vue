<template>
  <div class="root">
    <h2>Create Post</h2>
    <p>
      <input type="text" placeholder="Title" v-model="state.title" />
      <span v-if="v$.title.$error">
        {{ v$.title.$errors[0].$message }}
      </span>
    </p>
    <p>
      <input type="body" placeholder="Body" v-model="state.body" />
      <span v-if="v$.body.$error">
        {{ v$.body.$errors[0].$message }}
      </span>
    </p>
    <button @click="submitForm">Submit</button>
  </div>
</template>
<script>
import { reactive, computed } from "vue";
import useValidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import axios from "axios";
export default {
  name: "AboutView",
  components: {},
  setup() {
    const state = reactive({
      title: "",
      body: "",
    });

    const rules = computed(() => {
      return {
        title: {
          required,
        },
        body: {
          required,
        },
      };
    });
    const v$ = useValidate(rules, state);
    return { state, v$ };
  },
  data() {
    return {
      title: "",
      body: "",
      token: localStorage.getItem("userToken"),
    };
  },
  validations() {
    return {
      title: { required },
      body: { required },
    };
  },
  methods: {
    submitForm() {
      this.v$.$validate(); // checks all inputs
      if (!this.v$.$error) {
        axios
          .post(`http://127.0.0.1:8000/post/`, this.state, {
            headers: {
              Authorization: "Token " + this.token,
            },
          })
          .then((response) => {
            this.$router.push("/profile");
          })
          .catch((error) => {
            console.log(error);
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
</style>
