<template>
  <div class="d-flex">
    <!-- Barre latérale de navigation -->
    <nav class="navbar navbar-dark  flex-column p-3">
      <a class="navbar-brand mb-3 d-block" href="#">
        <img src="./styles/images/logoTempo.png" class="logo" alt="Logo">
      </a>

      <!-- Bouton de toggler pour les petits écrans (d-xl-none: masque le burger pour les grands écrans) -->
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="offcanvas"
        data-bs-target="#offcanvasNavbar"
        aria-controls="offcanvasNavbar"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <!-- Offcanvas Menu -->
      <div class="offcanvas offcanvas-start bg-dark" tabindex="-1" id="offcanvasNavbar" aria-labelledby="offcanvasNavbarLabel">
        <div class="offcanvas-header">
          <h5 class="offcanvas-title text-white" id="offcanvasNavbarLabel">Menu</h5>
          <button type="button" class="btn-close btn-close-white" data-bs-dismiss="offcanvas" aria-label="Close"></button>
        </div>
        <div class="offcanvas-body">
          <ul class="navbar-nav flex-column">
            <li class="nav-item">
              <a class="nav-link" href="#">Courses à faire</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Repas de la semaine</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Planning</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Tâches administratives</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- Contenu principal -->
    <main class="flex-grow-1 d-flex justify-content-center align-items-start">
      <div class="container">
        <h1 class="text-center mb-5"><i class="bi bi-cart-fill"></i> Liste de courses à faire</h1>
        <div>
          <form @submit.prevent="addItem" class="d-flex justify-content-center">
            <input style="width:300px" type="text" v-model="newItem" placeholder="Nom de l'article" />
            
             <button type="submit" class="btn bouton fw-bold mx-1 ">Ajouter un article</button>
          
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
    </main>
  </div>
</template>

<script setup>
import { ref } from "vue";

const courses = ref([
  { name: "Bananes", quantity: 1 },
  { name: "Haricots", quantity: 1 },
]);

const newItem = ref("");

const addItem = () => {
  if (newItem.value.trim() !== "") {
    courses.value.push({ name: newItem.value, quantity: 1 });
    newItem.value = "";
  }
};

const augmenter = (index) => {
  courses.value[index].quantity++;
};

const diminuer = (index) => {
  if (courses.value[index].quantity > 1) {
    courses.value[index].quantity--;
  } else {
    courses.value.splice(index, 1);
  }
};

const deleteItem = (index) => {
  courses.value.splice(index, 1);
};
</script>

<style>
.navbar {
  min-height: 100vh;
  background-color: #083731;
}

.navbar-nav .nav-link {
  color: #fff;
  padding: 10px 20px;
}

.navbar-nav .nav-link:hover {
  background-color: #62808b;
}

.bouton {
  background-color: #e9cbb1;
  color: #083731;
}

.bouton:hover {
  background-color: #083731;
  color: #fff;
}

.navbar-brand .logo{
  width: 100%;
  margin-left: 8px
}



@media (min-width: 768px) {
  .navbar {
    width: 20%;
  }

  .content-container {
    margin-left: 25%;
  }
}

@media (max-width: 1024px) {
  .navbar {
    position: static;
    width: 30%;
  }
}

@media (max-width: 500px) {
  .navbar-brand .logo {
    width: 170%;
    margin-left: -10px /* Agrandir le logo pour les petits écrans */
  }
  main{
    width: 80%;
  }

}

@media (max-width: 3000px){
  .navbar-toggler{
  margin-bottom: 600px;
  align-self: center;
}
}
</style>
