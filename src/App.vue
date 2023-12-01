<!-- src/App.vue -->

<template>
  <main class="w-full h-screen bg-opacity-75 flex justify-center items-center p-4">
    <div class="max-w-xl w-full bg-gray-700 bg-opacity-90 flex flex-col justify-center items-center py-10 rounded-3xl">
      <form @submit.prevent="submitForm" autocomplete="on" class="flex flex-col w-full text-white px-8">
        <label for="name" class="w-full text-2xl mb-2">Name:</label>
        <input type="text" class="text-lg text-black h-8 pl-2 mb-3 rounded-sm focus:outline-none focus:shadow-outline" placeholder="Ex: Hesham" v-model="formData.name" required>

        <label for="email" class="w-full text-2xl mb-2">Email:</label>
        <input type="email" id="email" class="text-lg text-black h-8 pl-2 mb-3 rounded-sm focus:outline-none focus:shadow-outline" placeholder="Ex: hesham@test.com" v-model="formData.email" required>

        <label for="country" class="w-full text-2xl mb-2">Country:</label>
        <CountryDropdown v-model="formData.country" />

        <button class=" focus:outline-none border border-transparent text-xl py-3 px-5 rounded-lg shadow-sm text-center text-social-500 hover:bg-white bg-transparent hover:text-gray-700 border-white font-medium" type="submit">
          <span v-if="!loading">
            Submit
          </span>
          <span v-if="loading" class="animate-spin inline-block w-6 h-6 border-4 border-solid border-r-transparent rounded-full"></span>
        </button>
        
      </form>
    </div>
  </main>

</template>

<script>
import CountryDropdown from './components/CountryDropdown.vue';

export default {
  data() {
    return {
      formData: {
        name: '',
        email: '',
        country: '',
      },
      loading: false,
    };
  },
  methods: {
    async submitForm() {
      this.loading = true;
      fetch('https://sellenvo.com/api/test', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json', 
        },
        body: this.formData,
      })
      .then(response => response.json())
      .then(data => {
        console.log(data);
        this.loading = false;
        this.$swal({
          title: "Submitted!",
          text: "We've received your data!",
          icon: "success"
        });
      })
      .catch(error => {
        console.error(error);
        this.loading = false;
        this.$swal({
          title: "Error!",
          text: "There is an error!",
          icon: "error",
          showConfirmButton: false,
          timer: 1500
        });
      });
    },
  },
  components: {
    CountryDropdown,
  },
};

</script>


<style scoped>
  main {
    background: url("/map.jpg");
    background-size: cover;
    background-position: center;
  }
</style>
