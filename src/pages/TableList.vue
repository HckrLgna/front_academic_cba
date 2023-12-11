<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title"> Question bank </h4>
              <p class="card-category">Table about total question bank </p>
            </template>
            <l-table class="table-hover table-striped"
                     :columns="table1.columns"
                     :data="table1.data"
                     :cell-renderers="cellRenderers">
                     <template slot="columns"> </template>

                <template slot-scope="{row}">

                  <td>{{row.id}}</td>
                  <td>{{row.topic}}</td>
                  <td>{{row.number_questions}}</td>

                  <td class="td-actions text-right">
                    <button type="button" class="btn-simple btn btn-xs btn-info" data-bs-toggle="modal" data-bs-target="#staticBackdrop" @click="editRow(row)" >
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
                                <div class="form-group">
                                  <label for="editTopic">Topic:</label>
                                  <input v-model="editFormData.topic" type="text" id="editTopic" class="form-control" required>
                                </div>

                                <div class="form-group">
                                  <label for="editQuestions">Preguntas:</label>
                                  <textarea v-model="editFormData.questions" id="editQuestions" class="form-control" rows="5" required></textarea>
                                </div>

                                <button type="submit" class="btn btn-primary">Guardar Cambios</button>
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

    <!-- Modal para editar -->



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
        columns: ['Id', 'Topic', 'number_questions', 'Options'],
        data: [],
      },
      editFormData: {
        id: null,
        topic: '',
        number_questions: '',
        questions: '',
      },
      // Otras variables de tooltips si es necesario
    };
  },
  methods: {
    editRow(row) {
      // Aquí puedes implementar la lógica para editar la fila
      console.log('Editar fila:', row);
      this.editFormData = {
        id: row.id,
        topic: row.topic,
        questions: row.questions,
      };

    },
    resetModal() {

      this.editFormData = {
        id: null,
        topic: '',
        questions: '',
      };
    },
    submitEditForm() {
      // Aquí puedes realizar la lógica para enviar la solicitud de edición al servidor
      // y manejar la respuesta.
      console.log('Guardar cambios:', this.editFormData);
      // Después de manejar la respuesta, cierra el modal
      this.resetModal();
    }
  },
  computed: {
    cellRenderers() {
      return {
        Actions: this.renderActionsCell,
      };
    },
  },
  mounted() {
    // Realizar la solicitud GET al nuevo endpoint
    axios.get('https://proyecto-sw2.up.railway.app/api/cuestionario')
      .then(response => {
        // Actualizar los datos de la tabla con la respuesta
        this.table1.data = response.data.map(question => ({
          id: question.id,
          topic: question.topico,
          number_questions: question.texto.split(';').length, // Contar la cantidad de preguntas
          questions: question.texto,

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
