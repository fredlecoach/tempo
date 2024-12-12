<template>
  <!-- Bouton pour basculer le menu (affiché uniquement sur les petits écrans) -->
  <button class="btn btn-primary d-lg-none" @click="toggleMenu">☰ Menu</button>

  <!-- Structure principale avec la barre latérale et le contenu -->
  <div class="d-flex">
    <!-- Barre latérale de navigation -->
    <div :class="['navbar', { hidden: !isMenuOpen }]">
      <span class="navbar-brand">
        <img src="./styles/images/logoTempo.png" style="width: 100%; max-width: 300px;" />
      </span>
      <ul class="navbar-nav list-unstyled">
        <li class="nav-item"><a class="nav-link" href="#">Courses à faire</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Repas de la semaine</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Planning</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Tâches administratives</a></li>
      </ul>
    </div>

    <!-- Contenu principal centré -->
    <div class="content-container flex-grow-1 d-flex justify-content-center align-items-start">
      <div class="container">
        <h1 class="text-center mb-5"><i class="bi bi-cart-fill"></i> Liste de courses à faire</h1>
        <div>
          <form @submit.prevent="addItem">
            <input style="width:300px" type="text" v-model="newItem" placeholder="Nom de l'article" />
            <button type="submit" class="btn bouton fw-bold mx-1">Ajouter un article</button>
          </form>
        </div>
        <hr>
        <ul class="list-unstyled">
          <li v-for="(item, index) in courses" :key="index" class="fw-bold">
            {{ item.name.toUpperCase() }} x{{ item.quantity }}
            <button class="btn btn-success my-2" @click="augmenter(index)"><i class="bi bi-bag-plus"></i></button>
            <button class="btn btn-warning mx-1 my-2" @click="diminuer(index)"><i class="bi bi-bag-dash-fill"></i></button>
            <button class="btn btn-danger" @click="deleteItem(index)">Supprimer</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

// Liste réactive des articles avec quantités
const courses = ref([
  { name: "Bananes", quantity: 1 },
  { name: "Haricots", quantity: 1 },
]);

// Nouvel article
const newItem = ref("");

// État pour afficher ou masquer le menu
const isMenuOpen = ref(true);
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

// Ajouter un article
const addItem = () => {
  if (newItem.value.trim() !== "") {
    courses.value.push({ name: newItem.value, quantity: 1 });
    newItem.value = ""; // Réinitialiser le champ de saisie
  }
};

// Augmenter la quantité d'un produit
const augmenter = (index) => {
  courses.value[index].quantity++;
};

// Diminuer la quantité d'un produit
const diminuer = (index) => {
  if (courses.value[index].quantity > 1) {
    courses.value[index].quantity--;
  } else {
    // Supprimer l'article si la quantité est 0
    courses.value.splice(index, 1);
  }
};

const deleteItem = (index) => {
  courses.value.splice(index, 1);
};
</script>

<style>
.navbar {
  background-color: #083731;
  color: #fff;
  position: fixed; /* Fixe le menu sur le côté gauche */
  top: 0;
  left: 0; /* Aligne le menu sur le bord gauche de l'écran */
  height: 100vh; /* Hauteur 100% de la fenêtre */
  width: 20%; /* Largeur du menu (ajustez selon vos besoins) */
  padding: 20px;
  display: flex;
  flex-direction: column; /* Organise les éléments du menu verticalement */
  transition: transform 0.3s ease-in-out;
}

.navbar.hidden {
  transform: translateX(-100%); /* Cache le menu */
}

.nav-link {
  color: #fff;
  padding: 10px 0; /* Espacement entre les liens */
  text-decoration: none; /* Enlever la décoration de lien */
}

.nav-link:hover {
  background-color: #458d93; /* Effet survol pour les liens */
  padding: 15px;
  color: #fff;
}

.navbar-brand {
  margin-bottom: 30px; /* Espacement sous le logo */
}

.bouton {
  background-color: #e9cbb1;
  color: #083731;
}

.bouton:hover {
  background-color: #083731;
  color: #fff;
}

.content-container {
  margin-left: 20%; /* Espace pour le menu latéral */
  padding: 20px; /* Espacement interne */
  min-height: 100vh; /* Assure une hauteur de la page complète */
}

/* Responsive pour les petits écrans */
@media (max-width: 480px) {
  .navbar {
    width: 100%; /* Utilise toute la largeur de l'écran */
    height: auto; /* Ajuste la hauteur */
  }

  .content-container {
    margin-left: 0; /* Pas de marge sur les petits écrans */
    padding-top: 200px; /* Laisse de l'espace pour la barre latérale si elle est en haut */
  }
}
</style>
