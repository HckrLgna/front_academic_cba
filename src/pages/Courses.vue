<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title">Módulos</h4>
              <p class="card-category">Tabla sobre los módulos</p>
            </template>
            <l-table class="table-hover table-striped"
                     :columns="table1.columns"
                     :data="table1.data"
                      >

            </l-table>
          </card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LTable from 'src/components/Table.vue';
import axios from 'axios';

export default {
  components: {
    LTable,
  },
  data() {
    return {
      table1: {
        columns: ['Id', 'Nombre', 'Nivel_ID', 'Options'],
        data: [],
      },
    };
  },
  mounted() {
    // Realizar la solicitud GET al nuevo endpoint
    axios.get('https://proyecto-sw2.up.railway.app/api/modulos')
      .then(response => {
        // Actualizar los datos de la tabla con la respuesta
        this.table1.data = response.data.map(modulo => ({
          id: modulo.id,
          nombre: modulo.nombre,
          nivel_id: 'BASIC',
            // Agrega la columna de opciones si es necesario
        }));
      })
      .catch(error => {
        console.error('Error al obtener datos:', error);
      });
  },
};
</script>

<style>
</style>
