<template>
  <div class="container">
    <div class="form-box">
      <h2>{{ isLogin ? "Connexion" : "Créer un compte" }}</h2>
      <form @submit.prevent="handleSubmit">
        <div class="input-group">
          <label>Email :</label>
          <input v-model="email" type="email" required />
        </div>

        <div class="input-group">
          <label>Mot de passe :</label>
          <input v-model="password" type="password" required />
        </div>

        <div class="input-group" v-if="!isLogin">
          <label>Confirmer le mot de passe :</label>
          <input v-model="confirmPassword" type="password" required />
        </div>

        <button type="submit">
          {{ isLogin ? "Se connecter" : "S'inscrire" }}
        </button>
      </form>

      <p class="toggle-text">
        {{ isLogin ? "Pas encore de compte ?" : "Déjà un compte ?" }}
        <span @click="toggleForm">{{ isLogin ? "S'inscrire" : "Se connecter" }}</span>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const isLogin = ref(true); // Mode actuel (connexion ou inscription)
const email = ref('');
const password = ref('');
const confirmPassword = ref('');

const toggleForm = () => {
  isLogin.value = !isLogin.value;
  email.value = '';
  password.value = '';
  confirmPassword.value = '';
};

const handleSubmit = () => {
  if (!isLogin.value && password.value !== confirmPassword.value) {
    alert("Les mots de passe ne correspondent pas !");
    return;
  }
  alert(isLogin.value ? "Connexion réussie !" : "Compte créé avec succès !");
};
</script>

<style scoped>
/* Centrage du formulaire */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f4f4f4;
}

/* Boîte du formulaire */
.form-box {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 300px;
  text-align: center;
}

/* Titre */
h2 {
  margin-bottom: 15px;
  color: #333;
}

/* Champs d'entrée */
.input-group {
  margin-bottom: 10px;
  text-align: left;
}

label {
  font-size: 14px;
  color: #555;
}

input {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
}

/* Bouton */
button {
  width: 100%;
  padding: 10px;
  border: none;
  background-color: #007bff;
  color: white;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background-color: #0056b3;
}

/* Texte bascule */
.toggle-text {
  margin-top: 10px;
  font-size: 14px;
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
