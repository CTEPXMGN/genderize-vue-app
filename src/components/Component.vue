<script>
const serverUrl = "https://api.genderize.io";

export default {
  name: "Component",
  data() {
    return {
      inputValue: "",
      name: "",
      gender: "...",
      minSymbols: 2,
    };
  },
  methods: {
    checkNameLength: function () {
      if (this.name.length < this.minSymbols) {
        alert("The name is too small");
      } else {
        this.getGender();
      }
    },
    getGender: async function () {
      const url = `${serverUrl}?name=${this.name}`;

      try {
        let response = await fetch(url);
        console.log("Пошёл запрос...");
        if (!response.ok) {
          console.log("Error" + response.status);
        } else {
          console.log("Запрос вернулся...");
          const result = await response.json();
          this.gender = `${this.upperCase(result.name)} is ${result.gender}`;
          this.name = "";
        }
      } catch (err) {
        console.log(err.message);
      }
    },
    upperCase: function (name) {
      return name[0].toUpperCase() + name.slice(1);
    },
  },
};
</script>

<template>
  <div class="gender__container">
    <form
      class="gender__form"
      action="submit"
      @submit.prevent="checkNameLength"
    >
      <input
        type="text"
        class="gender__input"
        placeholder="Input name..."
        v-model="name"
      />
      <button class="gender__btn">GET</button>
      <p class="gender__output">{{ gender }}</p>
    </form>
  </div>
</template>

<style scoped>
.gender__container {
  border: 2px solid #428f68;
  border-radius: 20px;
}

.gender__form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.gender__input {
  margin: 20px;
}

.gender__btn {
  margin: 10px;
}
</style>
