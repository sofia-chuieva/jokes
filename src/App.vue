<template>
  <section>
    <div class="container">
      <h1 class="font-extrabold text-4xl py-10">Joke of the day</h1>
      <div class="wrapper">
        <div class="header">
          <button
            id="dropdownCheckboxButton"
            data-dropdown-toggle="dropdownDefaultCheckbox"
            class="text-sky-800 w-full md:w-auto justify-between bg-white border-2 border-sky-700 hover:bg-gray-100 focus:ring-4 focus:outline-none font-medium rounded-lg text-md px-5 py-2.5 text-center inline-flex items-center"
            type="button"
          >
            Choose categories
            <svg
              class="w-2.5 h-2.5 ms-3"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 10 6"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m1 1 4 4 4-4"
              />
            </svg>
          </button>

          <!-- Dropdown menu -->
          <div
            id="dropdownDefaultCheckbox"
            class="z-10 hidden w-48 bg-white divide-y divide-gray-100 rounded-lg shadow"
          >
            <ul
              class="p-3 space-y-3 text-sm text-gray-700 dark:text-gray-200"
              aria-labelledby="dropdownCheckboxButton"
            >
              <li>
                <div class="flex items-center">
                  <input
                    :checked="isSelectedCategory('Miscellaneous')"
                    @change="toggleCategory('Miscellaneous')"
                    id="checkbox-item-1"
                    type="checkbox"
                    value="Miscellaneous"
                    class="w-4 h-4 text-sky-600 bg-gray-100 border-gray-300 rounded focus:ring-sky-500 dark:focus:ring-sky-600"
                  />
                  <label
                    for="checkbox-item-1"
                    class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                    >Misc</label
                  >
                </div>
              </li>
              <li>
                <div class="flex items-center">
                  <input
                    :checked="isSelectedCategory('Programming')"
                    @change="toggleCategory('Programming')"
                    id="checkbox-item-2"
                    type="checkbox"
                    value="Programming"
                    class="w-4 h-4 text-sky-600 bg-gray-100 border-gray-300 rounded focus:ring-sky-500 dark:focus:ring-sky-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500"
                  />
                  <label
                    for="checkbox-item-2"
                    class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                    >Programming</label
                  >
                </div>
              </li>
              <li>
                <div class="flex items-center">
                  <input
                    :checked="isSelectedCategory('Christmas')"
                    @change="toggleCategory('Christmas')"
                    id="checkbox-item-4"
                    type="checkbox"
                    value="Christmas"
                    class="w-4 h-4 text-sky-600 bg-gray-100 border-gray-300 rounded focus:ring-sky-500 dark:focus:ring-sky-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500"
                  />
                  <label
                    for="checkbox-item-4"
                    class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                    >Christmas</label
                  >
                </div>
              </li>
              <li>
                <div class="flex items-center">
                  <input
                    :checked="isSelectedCategory('Spooky')"
                    @change="toggleCategory('Spooky')"
                    id="checkbox-item-3"
                    type="checkbox"
                    value="Spooky"
                    class="w-4 h-4 text-sky-600 bg-gray-100 border-gray-300 rounded focus:ring-sky-500 dark:focus:ring-sky-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500"
                  />
                  <label
                    for="checkbox-item-3"
                    class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                    >Spooky</label
                  >
                </div>
              </li>
              <li>
                <div class="flex items-center">
                  <input
                    :checked="isSelectedCategory('Pun')"
                    @change="toggleCategory('Pun')"
                    id="checkbox-item-5"
                    type="checkbox"
                    value="Pun"
                    class="w-4 h-4 text-sky-600 bg-gray-100 border-gray-300 rounded focus:ring-sky-500 dark:focus:ring-sky-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500"
                  />
                  <label
                    for="checkbox-item-5"
                    class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                    >Pun</label
                  >
                </div>
              </li>
            </ul>
          </div>
        </div>

        <div class="my-8 text-xl">
          <p>
            {{ jokeSetup }}
          </p>
          <p class="text-sky-900 font-bold italic pt-4">{{ jokeDelivery }}</p>
        </div>
        <button @click="fetchData">Get new joke</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      jokeSetup: "",
      jokeDelivery: "",
      selectedCategories: ["Pun"],
    };
  },
  methods: {
    isSelectedCategory(category) {
      return this.selectedCategories.includes(category);
    },

    toggleCategory(category) {
      const index = this.selectedCategories.indexOf(category);

      if (index !== -1) {
        // Category is already selected, remove it
        this.selectedCategories.splice(index, 1);
      } else {
        // Category is not selected, add it
        this.selectedCategories.push(category);
      }
      this.toggleDropdown();
    },
    toggleDropdown() {
      const dropdown = document.getElementById("dropdownDefaultCheckbox");
      dropdown.classList.add("hidden");
    },
    fetchData() {
      const selectedCategoriesString = this.selectedCategories.join(",");

      fetch(
        `https://v2.jokeapi.dev/joke/${selectedCategoriesString}?blacklistFlags=nsfw,religious,political,racist,sexist,explicit&type=twopart`,
        {
          headers: {
            Accept: "application/json",
          },
        }
      )
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          this.jokeSetup = data.setup;
          this.jokeDelivery = data.delivery;
        });
    },
  },
  mounted() {
    // Fetch a random joke when the component is mounted
    this.fetchData();
  },
};
</script>
