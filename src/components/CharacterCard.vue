<template>
  <div class="character-card">
    <div class="image-container">
      <img :src="character.image" alt="character image" />
    </div>
    <div class="character-info">
      <div class="name">
        <h2>{{ character.name }}</h2>
        <div class="status">
          <span
            class="status-icon"
            :class="['status-icon', character.status.toLowerCase()]"
          ></span>
          <span>{{ character.status }} - {{ character.species }}</span>
        </div>
      </div>

      <div>
        <p class="location-label text-gray">Last known location:</p>
        <p class="location">{{ character.location.name }}</p>
      </div>
      <div>
        <p class="episode-label text-gray">First seen in:</p>
        <p class="episode">{{ character.firstEpisode }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  character: Object,
});
</script>

<style scoped>
.text-gray {
  color: rgb(158, 158, 158);
}
.character-card {
  display: flex;
  background-color: rgb(60, 62, 68);
  border-radius: 8px;
  overflow: hidden;
  color: #fff;
  width: 600px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s;
}

.image-container {
  height: 13rem;
  display: flex;
  align-items: center; /* Center vertically */
  justify-content: center; /* Center horizontally */
  overflow: hidden;
}

img {
  height: 100%;
  width: auto;
  object-fit: cover;
}

.character-info {
  padding: 16px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

h2 {
  margin: 0;
  font-size: 1.5em;
  color: #fff;
}

.status {
  display: flex;
  align-items: center;
}

.status-icon {
  height: 0.5rem;
  width: 0.5rem;
  margin-right: 0.375rem;
  border-radius: 50%;
}

.status-icon.alive {
  background: #55cc44;
}

.status-icon.dead {
  background: #d63d2e;
}

.status-icon.unknown {
  background: #9e9e9e;
}

.location-label,
.episode-label {
  font-weight: bold;
  font-size: medium;
  margin: 8px 0 4px;
  padding-bottom: 0.1rem;
}

.location,
.episode {
  margin: 0;
}
</style>
