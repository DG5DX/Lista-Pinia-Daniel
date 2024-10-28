<template>
    <q-page class="q-pa-md">
      <q-card>
        <q-card-section>
          <h2>Agregar Nueva Actividad</h2>
        </q-card-section>
  
        <q-form @submit="onSubmit" class="form-container">
          <q-input filled v-model="activityData.description" label="Descripción" lazy-rules class="input-field" />
          <q-input filled v-model="activityData.date" label="Fecha" type="date" lazy-rules class="input-field" />
          <q-select filled v-model="activityData.status" :options="statusOptions" label="Estado" class="input-field" />
          <div class="button-container">
            <q-btn label="Agregar" color="primary" type="submit" />
          </div>
        </q-form>
      </q-card>
    </q-page>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useAdminStore } from '../stores/admin.js';
  import { Notify } from 'quasar';
  
  const adminStore = useAdminStore();
  const activityData = ref({ description: '', date: '', status: '' });
  const statusOptions = ['Completado', 'Pendiente'];
  
  function onSubmit() {
    const missingFields = [];
    if (!activityData.value.description) missingFields.push('Descripción');
    if (!activityData.value.date) missingFields.push('Fecha');
    if (!activityData.value.status) missingFields.push('Estado');
  
    if (missingFields.length === 0) {
      adminStore.addActivity({ ...activityData.value });
      activityData.value = { description: '', date: '', status: '' };
      Notify.create({ message: 'Actividad agregada exitosamente', color: 'green' });
    } else {
      Notify.create({
        message: `Por favor completa los siguientes campos: ${missingFields.join(', ')}`,
        color: 'red'
      });
    }
  }
  </script>
  
  <style scoped>
  h2 {
    margin: 20px;
    text-align: center;
  }
  .form-container {
    padding: 20px;
  }
  .input-field {
    margin-bottom: 20px;
  }
  .button-container {
    display: flex;
    justify-content: flex-end;
    margin-top: 20px;
  }
  </style>
  