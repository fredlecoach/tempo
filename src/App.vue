<template>
  <!-- barre latérale de navigation -->
  <div>
    <div class="navbar">
      <span class="navbar-brand"><img src="./styles/images/logoTempo.png" style="width: 100%; max-width: 300px;" /></span>
      <ul class="navbar-nav list-unstyled">
        <li class="nav-item"><a class="nav-link" href="#">Courses à faire</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Repas de la semaine</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Planning</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Tâches administratives</a></li>
      </ul>
    </div>

    <!-- menu courses -->
    <div class="container">
      <h1 class="text-center mb-5"><i class="bi bi-cart-fill"></i> Liste de courses à faire</h1>
      <div>
        <form @submit.prevent="addItem">
          <input style="width:300px" type="text" v-model="newItem" placeholder="Nom de l'article" />
          <button type="submit" class="btn bouton fw-bold mx-1">Ajouter un article</button>
        </form>
      </div>
      <hr>
      <ul>
        <li v-for="(item, index) in courses" :key="index" class="fw-bold">
          {{ item.name.toUpperCase() }} ({{ item.quantity }})
          <button class="btn btn-success mx-1 my-2 fs-4" @click="augmenter(index)"><i class="bi bi-bag-plus"></i></button>
          <button class="btn btn-danger mx-1 my-2 fs-4" @click="diminuer(index)"><i class="bi bi-bag-dash-fill"></i></button>
        </li>
      </ul>
    </div>
  </div>
</template>


<script setup>
import { ref } from "vue";
import "./styles/css/navbar.css";

// Liste réactive des articles avec quantités
const courses = ref([
  { name: "Bananes", quantity: 1 },
  { name: "Haricots", quantity: 1 },
]);

// Nouvel article
const newItem = ref("");

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
</script>
