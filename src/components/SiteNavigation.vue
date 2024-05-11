<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">Lokalna pogoda</p>
        </div>
      </RouterLink>

      <div class="flex gap-3 flex-1 justify-end">
        <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
          @click="toggleModal"></i>
        <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer" @click="addCity"
          v-if="route.name != `home`"></i>

      </div>

      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">Opis:</h1>
          <p class="mb-4">
            Lokalna pogoda umożliwia śledzenie aktualnej i
            przyszła pogoda w wybranych przez Ciebie miastach.
          </p>
          <h2 class="text-2xl">
            Jak to działa:</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              Wyszukaj swoje miasto wpisując nazwę w polu
              pasek wyszukiwania.
            </li>
            <li>
              Wybierz miasto w wynikach, to zajmie
              do aktualnej pogody dla Twojego wyboru.
            </li>
            <li>
              Śledź miasto, klikając ikonę „+” w
              w prawym górnym rogu. Dzięki temu miasto będzie można oglądać o godz
              później na stronie głównej.
            </li>
          </ol>

          <h2 class="text-2xl">Usuwanie miasta</h2>
          <p>
            Jeśli nie chcesz już śledzić miasta, po prostu wybierz
            miasto na stronie głównej. Na dole
            stronie, będzie dostępna opcja usunięcia miasta.
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import { ref } from "vue";
import BaseModal from "./BaseModal.vue";

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );
  }

  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  savedCities.value.push(locationObj);
  localStorage.setItem(
    "savedCities",
    JSON.stringify(savedCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
};

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
