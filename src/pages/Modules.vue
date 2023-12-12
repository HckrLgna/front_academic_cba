<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title">Modules</h4>
              <p class="card-category">Table about modules</p>
            </template>
            <l-table class="table-hover table-striped"
                     :columns="table1.columns"
                     :data="table1.data"
                     :cell-renderers="cellRenderers">
                     <template slot="columns"> </template>

                <template slot-scope="{row}">

                  <td>{{row.id}}</td>
                  <td>{{row.nombre}}</td>
                  <td>{{row.nivel_id}}</td>

                  <td class="td-actions text-right">
                    <button type="button" class="btn-simple btn btn-xs btn-warning" data-bs-toggle="modal" data-bs-target="#staticBackdrop" @click="editRow(row)" >
                      <i class="nc-icon nc-bullet-list-67"></i>
                    </button>
                    <button type="button" class="btn-simple btn btn-xs btn-info"   >
                      <i class="fa fa-edit"></i>
                    </button>

                     <!-- Modal -->
                      <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                        <div class="modal-dialog">
                          <div class="modal-content">
                            <div class="modal-header">
                               <h5 class="modal-title" id="editModalLabel">Editar Pregunta</h5>
                              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                            </div>
                            <div class="modal-body">
                              <form @submit.prevent="submitEditForm">
                                <div class="col-md-12">
                                  <card>
                                    <template slot="header">
                                      <h5 class="title">Bank of question</h5>
                                      <p class="category">Backend development</p>
                                    </template>
                                    <l-table :data="tableData.data"
                                            :columns="tableData.columns">
                                      <template slot="columns"> </template>

                                      <template slot-scope="{row}">
                                        <td>
                                          <base-checkbox v-model="row.checked"></base-checkbox>
                                        </td>
                                        <td>{{row.title}}</td>

                                      </template>
                                    </l-table>

                                  </card>
                                </div>



                                <button type="submit" class="btn btn-primary" @click="notifyVue('top', 'center')">Guardar Cambios</button>
                              </form>
                            </div>

                          </div>
                        </div>
                      </div>

                    <button type="button" class="btn-simple btn btn-xs btn-danger" v-tooltip.top-center="row.deleteTooltip">
                      <i class="fa fa-times"></i>
                    </button>
                  </td>
                </template>
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
        columns: ['id', 'nombre', 'nivel_id', 'Options'],
        data: [],
      },
      tableData: {
          data: [
            {title: 'Sign contract for "What are conference organizers afraid of?"', checked: false},
            {title: 'Lines From Great Russian Literature? Or E-mails From My Boss?', checked: true},
            {
              title: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              checked: true
            },
            {title: 'Create 4 Invisible User Experiences you Never Knew About', checked: false},
            {title: 'Read "Following makes Medium better"', checked: false},
            {title: 'Unfollow 5 enemies from twitter', checked: false}
          ]
        }
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
          nivel_id: "Basic",

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
