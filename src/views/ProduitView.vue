<template>
  <main>
    <div>
      <table>
        <caption>Liste des produit</caption>
        <tr>
          <th>Nom</th>
          <th>Prix</th>
          <th>Stock</th>
          <th>Commande</th>
        </tr>
        <!-- Si le tableau des catégories est vide -->
        <tr v-if="!data.listeProduit">
          <td colspan="4">Veuillez patienter, chargement des produits...</td>
        </tr>
        <!-- Si le tableau des catégories n'est pas vide -->
        <tr v-for="produits in data.listeProduit" :key="produits.reference">
          <td>{{ produits.nom }}</td>
          <td>{{ produits.prixUnitaire }}</td>
          <td>{{ produits.unitesEnStock }}</td>
          <td>{{ produits.unitesCommandees }}</td>
        </tr>
        <td>
          <button @click="changePage('begin')">
            Premiere page
          </button>
        </td>
        <td>
          <button @click="changePage(-1)">
            Precedente
          </button>
        </td>
        <td>
          <button @click="changePage(1)">
            Suivante
          </button>
        </td>
        <td>
          <button @click="changePage('last')">
            Derniere page
          </button>
        </td>
      </table>
    </div>
  </main>
</template>

<script setup>
import {reactive, onMounted} from "vue";
import {BACKEND, doAjaxRequest} from "../api";

// Pour réinitialiser le formuaire
const ProduitVide = {
  nom: "",
  prixUnitaire: "",
  unitesEnStock: "",
  unitesCommandees: ""
};

let data = reactive({
  // Les données saisies dans le formulaire
  formulaireProduit: {...ProduitVide},
  // La liste des catégories affichée sous forme de table
  listeProduit: []
});

function changePage(page) {
  if(page === 'begin'){
    data.numeroPage = 0;
    chargeProduits();
  }
  else if(page == 'last'){
    data.numeroPage = data.pageMax;
    chargeProduits();
  }
  if(data.numeroPage + page >= 0 && data.numeroPage + page <= data.pageMax){
    if(data.numeroPage <= data.pageMax && data.numeroPage >= 0){
      data.numeroPage = data.numeroPage + page;
      chargeProduits();
    }
  }
}
function chargeProduits() {
  // Appel à l'API pour avoir la liste des catégories
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest(BACKEND + "/api/produits?sort=nom&page="date.page"&size=5")
      .then((json) => {
        data.listeProduit = json._embedded.produits;
      })
      .catch((error) => alert(error.message));
}



// A l'affichage du composant, on affiche la liste
onMounted(chargeProduits);

</script>


<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}


th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
</style>