<template>
  <div class="container">
    <div class="filters">
      <input v-model="filters.name" placeholder="Name" />
      <select v-model="filters.status">
        <option value="">Any Status</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Применить</button>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="!info.prev">Previous</button>
      <span>Page {{ page }}</span>
      <button @click="nextPage" :disabled="!info.next">Next</button>
    </div>
    <div class="character-grid">
      <CharacterCard
        v-for="character in characters"
        :key="character.id"
        :character="character"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import CharacterCard from "./CharacterCard.vue";

const characters = ref([]);
const page = ref(1);
const info = ref({});
const filters = ref({
  name: "",
  status: "",
});

const fetchFirstEpisode = async (character) => {
  if (character.episode.length > 0) {
    const response = await axios.get(character.episode[0]);
    return {
      ...character,
      firstEpisode: response.data.name,
    };
  }
  return {
    ...character,
    firstEpisode: "Unknown",
  };
};

const fetchCharacters = async () => {
  const { data } = await axios.get(
    "https://rickandmortyapi.com/api/character",
    {
      params: {
        page: page.value,
        name: filters.value.name,
        status: filters.value.status,
      },
    }
  );
  const characterData = await Promise.all(
    data.results.map(async (character) => {
      return await fetchFirstEpisode(character);
    })
  );
  characters.value = characterData;
  info.value = data.info;
};

const applyFilters = () => {
  page.value = 1;
  fetchCharacters();
};

const nextPage = () => {
  if (info.value.next) {
    page.value++;
    fetchCharacters();
  }
};

const prevPage = () => {
  if (info.value.prev) {
    page.value--;
    fetchCharacters();
  }
};

onMounted(fetchCharacters);
</script>

<style scoped>
.container {
  padding: 20px;
}

.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.character-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  justify-items: center;
  padding: 0 20px;
}

@media (max-width: 768px) {
  .character-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .character-grid {
    grid-template-columns: 1fr;
  }
}
</style>
