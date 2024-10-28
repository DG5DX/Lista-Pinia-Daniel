<template>
    <div class="q-pa-md">
      <q-card>
        <q-card-section>
          <h2>Editar Actividad</h2>
        </q-card-section>
  
        <q-form @submit="onSubmit" class="form-container">
          <q-input filled v-model="activity.description" label="Descripción" lazy-rules class="input-field" />
          <q-input filled v-model="activity.date" label="Fecha" type="date" lazy-rules class="input-field" />
          <q-select filled v-model="activity.status" :options="statusOptions" label="Estado" class="input-field" />
          <div class="button-container">
            <q-btn label="Guardar Cambios" color="primary" type="submit" />
          </div>
        </q-form>
      </q-card>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useAdminStore } from '../stores/admin.js';
  import { Notify } from 'quasar';
  import { useRouter } from 'vue-router';
  
  const store = useAdminStore();
  const router = useRouter();
  const activity = ref({ description: '', date: '', status: '' });
  const statusOptions = ['Completado', 'Pendiente'];
  
  onMounted(() => {
    const selected = store.selectedActivity;
    if (selected) {
      activity.value = { ...selected };
    }
  });
  
  function onSubmit() {
    const missingFields = [];
    if (!activity.value.description) missingFields.push('Descripción');
    if (!activity.value.date) missingFields.push('Fecha');
    if (!activity.value.status) missingFields.push('Estado');
  
    if (missingFields.length === 0) {
      store.updateActivity(activity.value);
      Notify.create({ message: 'Actividad actualizada exitosamente', color: 'green' });
      setTimeout(() => {
        router.push('/');
      }, 500);
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