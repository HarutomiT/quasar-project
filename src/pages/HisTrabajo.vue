<template>

  <q-toolbar class="column">
      <q-input
        style="width: 100%;"
        filled
        class="custom-input"
        v-model="search"
        label="Busqueda"
      >
         <template v-slot:prepend>
            <q-btn flat round dense class="icono_de_busqueda" icon="search"/>
         </template>
      </q-input>

      <q-toolbar class="row reverse">
         <q-btn flat bordered>
            <q-avatar icon="mdi-plus-circle-outline" />
         </q-btn>
  </q-toolbar>

   </q-toolbar>

   <div class="q-pa-md">
    <q-btn-dropdown color="#a5a5a5" label="Trabajos" text-color="#000000">
      <q-list>
        <q-item clickable v-close-popup @click="onItemClick">
          <q-item-section>
            <q-item-label>Rechazados</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable v-close-popup @click="onItemClick">
          <q-item-section>
            <q-item-label>Aceptados</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-btn-dropdown>
  </div>

  <q-page>
    <div v-if="selectedOption === 'Aceptados'"class="q-mx-auto">
    <q-list bordered>
      <q-item>
        <q-item-section class="col-2">
          <q-item-label header>Nombre Empresa</q-item-label>
        </q-item-section>
        <q-item-section class="col-2">
          <q-item-label header>Trabajo a realizar</q-item-label>
        </q-item-section>
        <q-item-section class="col-2 text-center">
          <q-item-label header>Fecha</q-item-label>
        </q-item-section>

        <q-item-section class="col-4 text-center">
          <q-item-label header>Confirmación</q-item-label>
        </q-item-section>
        <q-item-section class="col-1 text-center">
          <q-item-label header>Más información</q-item-label>
        </q-item-section>

      </q-item>

      <q-item v-for="(actividad, index) in actividadesFiltradas" :key="index" :class="{'bg-grey-4': index % 2 === 0}">
        <q-item-section class="col-3 text center">
          {{ actividad.nombre }}
        </q-item-section>
        <q-item-section class="col-2">
          {{ actividad.trabajo }}
        </q-item-section>
        <q-item-section class="col-2 text-center">
          {{ actividad.fecha }}
        </q-item-section>

        <q-item-section class="col-3 text-center">
          <div v-if="actividad.entradaConfirmada && actividad.salidaConfirmada" style="display: flex; align-items: center; justify-content: center;">
            Hora Confirmada: {{ actividad.entradaConfirmada }} - {{ actividad.salidaConfirmada }}
          </div>
        </q-item-section>
        <q-item-section class="col-2 text-center">
          {{ actividad.MásInformación }}
        <div class="q-pa-md q-gutter-sm">
         <q-btn color="white" text-color="black" icon="description" />
        </div>
        </q-item-section>
      </q-item>
    </q-list>
   </div>
  </q-page>

  <q-page>
    <div v-if="selectedOption === 'Rechazados'"class="q-mx-auto">
    <q-list bordered>
      <q-item>
        <q-item-section class="col-2">
          <q-item-label header>Nombre Empresa</q-item-label>
        </q-item-section>
        <q-item-section class="col-2">
          <q-item-label header>Trabajo a realizar</q-item-label>
        </q-item-section>
        <q-item-section class="col-2 text-center">
          <q-item-label header>Fecha</q-item-label>
        </q-item-section>

        <q-item-section class="col-4 text-center">
          <q-item-label header>Confirmación</q-item-label>
        </q-item-section>
        <q-item-section class="col-1 text-center">
          <q-item-label header>Más información</q-item-label>
        </q-item-section>

      </q-item>

      <q-item v-for="(actividad, index) in actividadesFiltradas" :key="index" :class="{'bg-grey-4': index % 2 === 0}">
        <q-item-section class="col-3 text center">
          {{ actividad.nombre }}
        </q-item-section>
        <q-item-section class="col-2">
          {{ actividad.trabajo }}
        </q-item-section>
        <q-item-section class="col-2 text-center">
          {{ actividad.fecha }}
        </q-item-section>
        <q-item-section class="col-2 text-center">
          {{ actividad.MásInformación }}
        <div class="q-pa-md q-gutter-sm">
         <q-btn color="white" text-color="black" icon="description" />
        </div>
        </q-item-section>
      </q-item>
    </q-list>
   </div>
  </q-page>

</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const actividades = ref([
  { nombre: "Los Pinos", trabajo: "Revisar sistema eléctrico", fecha: "2024-010-11", completada: false },
  { nombre: "El Roble", trabajo: "Reparar sistema de agua", horaEntrada: "9:00", horaSalida: "18:00", fecha: "2024-08-2", completada: false },
  { nombre: "Industria Metalúrgica", trabajo: "Mantenimiento de máquinas", horaEntrada: "7:30", horaSalida: "16:00", fecha: "2024-08-25", completada: false },
  { nombre: "Colegio San Martín", trabajo: "Instalación de paneles solares", horaEntrada: "7:00", horaSalida: "15:00", fecha: "2024-08-26", completada: false },
  { nombre: "Hospital Central", trabajo: "Reparación de generador", horaEntrada: "6:30", horaSalida: "14:30", fecha: "2024-08-27", completada: false },
  { nombre: "Taller Mecánico López", trabajo: "Reemplazo de luces fluorescentes", horaEntrada: "10:00", horaSalida: "19:00", fecha: "2024-08-28", completada: false },
  { nombre: "Fábrica de Plásticos", trabajo: "Inspección de seguridad", horaEntrada: "8:30", horaSalida: "17:30", fecha: "2024-08-29", completada: false },
  { nombre: "Universidad Nacional", trabajo: "Revisión de red eléctrica", horaEntrada: "7:45", horaSalida: "16:15", fecha: "2024-08-30", completada: false },
  { nombre: "Estación de Bomberos", trabajo: "Mantenimiento de equipo", horaEntrada: "7:00", horaSalida: "15:30", fecha: "2024-08-31", completada: false },
  { nombre: "Centro Comercial Las Flores", trabajo: "Instalación de cámaras de seguridad", horaEntrada: "9:00", horaSalida: "18:00", fecha: "2024-09-2", completada: false },
  { nombre: "Los Pinos", trabajo: "Revisar sistema eléctrico", horaEntrada: "8:00", horaSalida: "17:00", fecha: "2024-08-2", completada: false },
  { nombre: "El Roble", trabajo: "Reparar sistema de agua", horaEntrada: "9:00", horaSalida: "18:00", fecha: "2024-08-2", completada: false },
  { nombre: "Industria Metalúrgica", trabajo: "Mantenimiento de máquinas", horaEntrada: "7:30", horaSalida: "16:00", fecha: "2024-09-25", completada: false },
  { nombre: "Colegio San Martín", trabajo: "Instalación de paneles solares", horaEntrada: "7:00", horaSalida: "15:00", fecha: "2024-08-26", completada: false },
  { nombre: "Hospital Central", trabajo: "Reparación de generador", horaEntrada: "6:30", horaSalida: "14:30", fecha: "2024-08-27", completada: false },
  { nombre: "Taller Mecánico López", trabajo: "Reemplazo de luces fluorescentes", horaEntrada: "10:00", horaSalida: "19:00", fecha: "2024-08-28", completada: false },
  { nombre: "Fábrica de Plásticos", trabajo: "Inspección de seguridad", horaEntrada: "8:30", horaSalida: "17:30", fecha: "2024-08-29", completada: false },
  { nombre: "Universidad Nacional", trabajo: "Revisión de red eléctrica", horaEntrada: "7:45", horaSalida: "16:15", fecha: "2024-08-30", completada: false },
  { nombre: "Estación de Bomberos", trabajo: "Mantenimiento de equipo", horaEntrada: "7:00", horaSalida: "15:30", fecha: "2024-08-31", completada: false },
  { nombre: "Centro Comercial Las Flores", trabajo: "Instalación de cámaras de seguridad", horaEntrada: "9:00", horaSalida: "18:00", fecha: "2024-09-01", completada: false },
  { nombre: "Los Pinos", trabajo: "Revisar sistema eléctrico", horaEntrada: "8:00", horaSalida: "17:00", fecha: "2024-08-2", completada: false },
  { nombre: "El Roble", trabajo: "Reparar sistema de agua", horaEntrada: "9:00", horaSalida: "18:00", fecha: "2024-08-2", completada: false },
  { nombre: "Industria Metalúrgica", trabajo: "Mantenimiento de máquinas", horaEntrada: "7:30", horaSalida: "16:00", fecha: "2024-08-25", completada: false },
  { nombre: "Colegio San Martín", trabajo: "Instalación de paneles solares", horaEntrada: "7:00", horaSalida: "15:00", fecha: "2024-08-26", completada: false },
  { nombre: "Hospital Central", trabajo: "Reparación de generador", horaEntrada: "6:30", horaSalida: "14:30", fecha: "2024-08-27", completada: false },
  { nombre: "Taller Mecánico López", trabajo: "Reemplazo de luces fluorescentes", horaEntrada: "10:00", horaSalida: "19:00", fecha: "2024-08-28", completada: false },
  { nombre: "Fábrica de Plásticos", trabajo: "Inspección de seguridad", horaEntrada: "8:30", horaSalida: "17:30", fecha: "2024-08-29", completada: false },
  { nombre: "Universidad Nacional", trabajo: "Revisión de red eléctrica", horaEntrada: "7:45", horaSalida: "16:15", fecha: "2024-08-30", completada: false },
  { nombre: "Estación de Bomberos", trabajo: "Mantenimiento de equipo", horaEntrada: "7:00", horaSalida: "15:30", fecha: "2024-08-31", completada: false },
  { nombre: "Centro Comercial Las Flores", trabajo: "Instalación de cámaras de seguridad", horaEntrada: "9:00", horaSalida: "18:00", fecha: "2024-09-01", completada: false }
]);

const actividadesFiltradas = computed(() => {
  const hoy = new Date().toISOString().split('T')[0];
  return actividades.value.filter(actividad => actividad.fecha >= hoy);
});

</script>

<style scoped>


@media only screen and (max-width: 1022px) {
.ola{
  overflow-x: scroll;
  width: 900px;
}
}


</style>
<style>
.botonar {
  color: #000000;
}
</style>
