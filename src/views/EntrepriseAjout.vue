<template>
  <div class="ajouter-entreprise-container">
    <div class="entreprise-header-card">
      <div class="nom-container">
        <h2 class="entreprise-nom">Mettre à jour une entreprise</h2>
      </div>
    </div>
    <form @submit.prevent="mettreAJourEntreprise" class="entreprise-detail-wrapper">
      <div class="entreprise-info-card">
        <div class="form-group">
          <label for="nom">Entreprise</label>
          <input id="nom" type="text" v-model="entreprise.name" />
        </div>

        <div class="form-group">
          <label for="logo">Logo (URL)</label>
          <input id="logo" type="text" v-model="entreprise.image" />
        </div>
      </div>

      <div class="section">
        <h2 class="section-title"><span>Courte présentation</span></h2>
        <textarea id="presentation" v-model="entreprise.description"></textarea>
      </div>

      <div class="section">
        <h3 class="section-title">Informations de contact</h3>
        <div class="grid-container">
          <div class="form-group">
            <label for="adresse">Adresse</label>
            <input id="adresse" type="text" v-model="entreprise.address" />
          </div>
          <div class="form-group">
            <label for="telephone">Téléphone</label>
            <input id="telephone" type="tel" v-model="entreprise.phone" />
          </div>
          <div class="form-group">
            <label for="ville">Ville</label>
            <input id="ville" type="text" v-model="entreprise.city" />
          </div>
          <div class="form-group">
            <label for="courriel">Courriel</label>
            <input id="courriel" type="email" v-model="entreprise.email" />
          </div>
          <div class="form-group">
            <label for="province">Province</label>
            <input id="province" type="text" v-model="entreprise.province.value" />
          </div>
          <div class="form-group">
            <label for="cp">Code postal</label>
            <input id="cp" type="text" v-model="entreprise.postalCode" />
          </div>
        </div>
      </div>
      <div class="form-buttons">
        <button type="button" class="cancel-button" @click="$router.go(-1)">Annuler</button>
        <button type="submit" class="save-button"><i class="fas fa-save"></i> Sauvegarder</button>
      </div>
    </form>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { useEntreprise } from '@/composables/entreprises/entreprise';

export default {
  name: 'EntrepriseMiseAjour',
  setup() {
    const route = useRoute();
    const router = useRouter();
    const { chargerEntreprise, mettreAjourEntreprise } = useEntreprise();
    const entreprise = ref({
      name: '',
      image: '',
      description: '',
      address: '',
      phone: '',
      city: '',
      email: '',
      province: { value: '' },
      postalCode: ''
    });

    onMounted(async () => {
      if (route.params.id) {
        const data = await chargerEntreprise(route.params.id);
        entreprise.value = data;
      }
    });

    const mettreAJourEntreprise = async () => {
      await mettreAjourEntreprise(route.params.id, entreprise.value);
      router.push({ name: 'Entreprises' });
    };

    return {
      entreprise,
      mettreAJourEntreprise
    };
  }
};
</script>


<style scoped>
.ajouter-entreprise-container {
  width: 90%;
  margin: auto;
  padding: 1rem;
}

.entreprise-header-card {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 1rem;
}

.nom-container {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-left: 5px solid #89a9e6;
  padding-left: 1rem;
}
.entreprise-info-card {
  margin-bottom: 20px;
  width: 80%;
  padding: 1rem 0;
}
.entreprise-nom {
  color: gray;
  font-weight: bold;
  font-size: 30px;
  margin: 0;
  margin-bottom: 2rem;
}

.entreprise-detail-wrapper {
  position: relative;
  padding: 1rem;
}

.entreprise-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
}

.section-title span {
  font-weight: bold;
  color: #89a9e6;
  font-size: 20px;
}
.section-title {
  color: #89a9e6;
}
.info-item {
  border-left: 8px solid grey;
  padding-left: 10px;
  width: 50%;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.form-group,
.file-upload-group {
  margin-bottom: 1rem;
}

.info-item label {
  font-weight: bold;
  color: gray;
}

input,
textarea {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-top: 0.5rem;
}

.file-input {
  border: none;
}
.form-buttons {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-right: 80px;
  margin-bottom: 50px;
}

.cancel-button,
.save-button {
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cancel-button {
  background-color: white;
  border: 1px solid gray;
  color: gray;
}

.save-button {
  background-color: #89a9e6;
  color: white;
}

.save-button i.fas.fa-save {
  margin-right: 8px;
}
</style>
