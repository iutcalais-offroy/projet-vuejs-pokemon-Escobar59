<template>
  <div>
    <!-- Champ de recherche -->
    <n-input v-model="searchQuery" placeholder="Recherchez un Pokémon..." style="margin-bottom: 20px;" />

    <n-card v-if="filteredPokemons.length > 0" title="Résultat de la recherche">
      <n-space wrap align="center" justify="center">
        <!-- Affiche uniquement le Pokémon filtré -->
        <n-card
          v-for="pokemon in filteredPokemons"
          :key="pokemon.id"
          :style="{ width: '200px', margin: '10px' }"
          :title="pokemon.name"
          hoverable
        >
          <template #header>
            <div style="text-align: center;">
              <img :src="pokemon.image" alt="pokemon.name" style="width: 100px; height: 100px;" />
            </div>
          </template>
          <div>
            <p><strong>PV:</strong> {{ pokemon.pv }}</p>
            <p><strong>Type:</strong> {{ pokemon.type }}</p>
            <p><strong>Taille:</strong> {{ pokemon.taille }}</p>
            <p><strong>Poids:</strong> {{ pokemon.poids }}</p>
            <p><strong>Attaque:</strong> {{ pokemon.attaque.nom }} ({{ pokemon.attaque.pv }} PV)</p>
          </div>
        </n-card>
      </n-space>
    </n-card>

    <!-- Message si aucun Pokémon trouvé -->
    <n-card v-else title="Aucun Pokémon trouvé">
      <p>Votre recherche n'a produit aucun résultat. Essayez un autre nom.</p>
    </n-card>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue';

const pokemons = ref([
  {
    id: 1,
    name: 'Bulbizarre',
    image: 'bulbasaur.png', 
    pv: 45,
    type: 'Plante',
    taille: '0.7m',
    poids: '6.9kg',
    attaque: { nom: 'Charge', pv: 10 }
  },
  {
    id: 2,
    name: 'Salamèche',
    image: 'charmander.png',
    pv: 39,
    type: 'Feu',
    taille: '0.6m',
    poids: '8.5kg',
    attaque: { nom: 'Flammèche', pv: 30 }
  },
  {
    id: 3,
    name: 'Carapuce',
    image: 'squirtle.png',
    pv: 44,
    type: 'Eau',
    taille: '0.5m',
    poids: '9kg',
    attaque: { nom: 'Pistolet à O', pv: 30 }
  },
  {
    id: 4,
    name: 'Pikachu',
    image: 'pikachu.png',
    pv: 35,
    type: 'Électrique',
    taille: '0.4m',
    poids: '6kg',
    attaque: { nom: 'Charge', pv: 10 }
  }
]);

const searchQuery = ref('');

const filteredPokemons = computed(() =>
  pokemons.value.filter(pokemon =>
    pokemon.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
);
</script>

<style scoped>
/* Styles inchangés */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f8ff;
}

.form-box {
  background: #ffffff;
  padding: 25px;
  border-radius: 30px; 
  box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2); 
  width: 320px;
  text-align: center;
  border: 2px solid #dcdfe6; 
}

h2 {
  margin-bottom: 20px;
  color: #2c3e50; 
  font-size: 24px;
  font-weight: bold;
}

.input-group {
  margin-bottom: 15px;
  text-align: left;
}

label {
  font-size: 16px;
  color: #34495e; 
  font-weight: 500;
}

input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #bdc3c7; 
  border-radius: 8px;
  font-size: 16px;
  background-color: #fdfdfd; 
  transition: border-color 0.3s;
}

input:focus {
  border-color: #007bff; 
  outline: none;
}

button {
  width: 100%;
  padding: 12px;
  border: none;
  background-color: #007bff;
  color: white;
  font-size: 18px;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s, transform 0.2s;
}

button:hover {
  background-color: #0056b3; 
  transform: translateY(-2px); 
}

button:active {
  transform: translateY(0); 
}

.toggle-text {
  margin-top: 15px;
  font-size: 14px;
  color: #34495e;
}

.toggle-text span {
  color: #007bff;
  cursor: pointer;
  font-weight: bold;
}

.toggle-text span:hover {
  text-decoration: underline;
}
</style>
