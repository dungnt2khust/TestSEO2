<template>
  <div id="app">
    <div class="loading"></div>
    <div>Type your password</div>
    <input
      style="margin-top: 20px"
      :class="{ invalid: passwordInvalid }"
      type="text"
      v-model="password"
      @change="validatePassword"
    />
    <div v-if="passwordInvalid" class="error">{{ errorMessage }}</div>
    <div style="margin-top: 20px">
      <button @click="validatePassword">Submit</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      cssTexts: [],
      password: "",
      passwordInvalid: false,
      errorMessage: "",
    };
  },
  metaInfo: {
    style: [
      {
        vmid: "page-load-overlay",
        innerHTML: `
          body div.loading {
            z-index: 999;
            background-color: yellow;
            opacity: 0.9;
          }
        `,
      },
    ],
  },
  mounted() {
    this.cssTexts.push({
      vmid: "page-load-overlay",
      innerHTML: null,
    });
  },
  methods: {
    validatePassword() {
      // Clear lỗi
      this.passwordInvalid = false;
      this.errorMessage = "";

      // Không được nhỏ hơn 6 kí tụ
      if (this.password.length < 6) {
        this.passwordInvalid = true;
        this.errorMessage = "Không được ít hơn 6 kí tự";
      }

      // Phải có chữ số và ký tự viết hoa
      var haveDigit = false,
        haveUpperLetter = false;
      for (var i = 0; i < this.password.length; i++) {
        // Có số
        if (!isNaN(this.password[i])) haveDigit = true;
        // Có kí tự viết hoa
        else if (this.password[i] === this.password[i].toString().toUpperCase())
          haveUpperLetter = true;
      }

      if (!haveDigit && !this.passwordInvalid) {
        this.passwordInvalid = true;
        this.errorMessage = "Phải có chữ số";
      }

      if (!haveUpperLetter && !this.passwordInvalid) {
        this.passwordInvalid = true;
        this.errorMessage = "Phải có ký tự in hoa";
      }
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.invalid {
  border: 1px solid red;
}
</style>
