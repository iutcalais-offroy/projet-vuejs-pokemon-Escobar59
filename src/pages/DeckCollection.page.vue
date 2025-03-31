<template>
  <div>
    <!-- Champ de recherche -->
    <n-input v-model="searchQuery" placeholder="Recherchez un Pokémon..." @keydown.enter.prevent style="margin-bottom: 20px;" />

    <n-card v-if="filteredPokemons.length > 0" title="Résultat de la recherche">
      <n-space wrap align="center" justify="center">
        <!-- Affiche les Pokémon filtrés -->
        <n-card
          v-for="pokemon in filteredPokemons"
          :key="pokemon.id"
          :style="{ width: '200px', margin: '10px' }"
          :title="pokemon.name"
          hoverable
        >
          <template #header>
            <div style="text-align: center;">
              <img :src="pokemon.image" :alt="pokemon.name" style="width: 100px; height: 100px;" />
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
      <p>Votre recherche n'a produit aucun résultat. Essayez un autre nom ou type.</p>
    </n-card>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue';

const pokemons = ref([
  { id: 1, name: 'Bulbizarre', image: 'bulbasaur.png', pv: 45, type: 'Plante', taille: '0.7m', poids: '6.9kg', attaque: { nom: 'Charge', pv: 10 } },
  { id: 2, name: 'Salamèche', image: 'charmander.png', pv: 39, type: 'Feu', taille: '0.6m', poids: '8.5kg', attaque: { nom: 'Flammèche', pv: 30 } },
  { id: 3, name: 'Carapuce', image: 'squirtle.png', pv: 44, type: 'Eau', taille: '0.5m', poids: '9kg', attaque: { nom: 'Pistolet à O', pv: 30 } },
  { id: 4, name: 'Pikachu', image: 'pikachu.png', pv: 35, type: 'Électrique', taille: '0.4m', poids: '6kg', attaque: { nom: 'Charge', pv: 10 } }
]);

const searchQuery = ref('');

const filteredPokemons = computed(() => {
  const query = searchQuery.value.toLowerCase().trim();
  return query
    ? pokemons.value.filter(pokemon => pokemon.name.toLowerCase().includes(query))
    : pokemons.value;
});
</script>

<style scoped>
/* Styles inchangés */
</style>
