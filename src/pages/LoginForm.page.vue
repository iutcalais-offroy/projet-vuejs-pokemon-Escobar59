<script setup>
import { ref, computed, onMounted } from 'vue';

const isLogin = ref(true);
const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const isAuthenticated = ref(false);

// Vérification du token au chargement
onMounted(() => {
  const token = localStorage.getItem("userToken");
  isAuthenticated.value = !!token;
});

// Génère un token unique
const generateToken = () => {
  return Math.random().toString(36).substring(2) + Math.random().toString(36).substring(2);
};

// Gère la connexion ou l'inscription
const handleSubmit = () => {
  if (!email.value || !password.value) {
    alert("Veuillez remplir tous les champs !");
    return;
  }

  if (!isLogin.value && password.value !== confirmPassword.value) {
    alert("Les mots de passe ne correspondent pas !");
    return;
  }

  const userId = email.value; // Utilisation de l'email comme ID
  const token = generateToken();

  // Stockage en local
  localStorage.setItem("userToken", token);
  localStorage.setItem("userId", userId);

  isAuthenticated.value = true;
  alert(isLogin.value ? "Connexion réussie !" : "Compte créé avec succès !");
};

// Déconnexion
const logout = () => {
  localStorage.removeItem("userToken");
  localStorage.removeItem("userId");

  isAuthenticated.value = false;
  email.value = "";
  password.value = "";
  confirmPassword.value = "";

  alert("Déconnexion réussie !");
};

// Récupération de l'utilisateur connecté
const userId = computed(() => localStorage.getItem("userId") || "Utilisateur");

// Basculer entre connexion et inscription
const toggleForm = () => {
  isLogin.value = !isLogin.value;
  email.value = '';
  password.value = '';
  confirmPassword.value = '';
};
</script>

<template>
  <div class="container">
    <div v-if="!isAuthenticated" class="form-box">
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

    <div v-else class="form-box">
      <h2>Bienvenue, {{ userId }} !</h2>
      <p>Vous êtes connecté 🎉</p>
      <button @click="logout">Se déconnecter</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f4f4f4;
}

.form-box {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 300px;
  text-align: center;
}

h2 {
  margin-bottom: 15px;
  color: #333;
}

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
