<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <div class="row">
                <div class="col md-11">
                  <h4 class="card-title">Total Students</h4>
                  <p class="card-category">Table about the number total students</p>
                </div>
                <div class="col md-1 text-right">
                  <router-link :to="{path:'/register'}" class="btn btn-primary">Add Student</router-link>
                </div>


              </div>


            </template>
            <l-table class="table-hover table-striped"
                     :columns="table1.columns"
                     :data="table1.data">
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
        columns: ['Id', 'Nombre', 'Correo', 'Telefono', 'Tipo'],
        data: [],
      },
    };
  },
  mounted() {
    // Realizar la solicitud GET al endpoint
    axios.get('https://proyecto-sw2.up.railway.app/api/usuarios')
      .then(response => {
        // Actualizar los datos de la tabla con la respuesta
        this.table1.data = response.data.map(user => ({
          id: user.id,
          nombre: user.nombre,
          correo: user.correo,
          telefono: user.telefono,
          tipo: user.tipo,
          options: 'edit', // Agrega la columna de opciones si es necesario
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
