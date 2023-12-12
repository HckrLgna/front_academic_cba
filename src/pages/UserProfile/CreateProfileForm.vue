<template>
  <card>
    <h4 slot="header" class="card-title">Register  Profile</h4>
    <form>
      <div class="row">
        <div class="col-md-5">
          <base-input type="text"
                    label="Role"
                    :disabled="true"
                    placeholder="Student"
                    v-model="user.role">
          </base-input>
        </div>

        <div class="col-md-7">
          <base-input type="email"
                    label="Email"
                    placeholder="Email"
                    v-model="user.email">
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
          <base-input type="text"
                    label="First Name"
                    placeholder="First Name"
                    v-model="user.firstName">
          </base-input>
        </div>
        <div class="col-md-6">
          <base-input type="text"
                    label="Last Name"
                    placeholder="Last Name"
                    v-model="user.lastName">
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-12">
          <base-input type="text"
                    label="Address"
                    placeholder="Home Address"
                    v-model="user.address">
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-4">
          <base-input type="text"
                    label="City"
                    placeholder="City"
                    v-model="user.city">
          </base-input>
        </div>
        <div class="col-md-4">
          <base-input type="text"
                    label="Country"
                    placeholder="Country"
                    v-model="user.country">
          </base-input>
        </div>
        <div class="col-md-4">
          <base-input type="number"
                    label="Phone number"
                    placeholder="+591"
                    v-model="user.phoneNumber">
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-12">
          <div class="form-group">
            <label>About Me</label>
            <textarea rows="5" class="form-control border-input"
                      placeholder="Here can be your description"
                      v-model="user.aboutMe">
              </textarea>
          </div>
        </div>
      </div>
      <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="createProfile">
          Register Profile
        </button>
      </div>
      <div class="clearfix"></div>
    </form>
  </card>
</template>
<script>
  import Card from 'src/components/Cards/Card.vue'
  import axios  from 'axios'
  export default {
    components: {
      Card
    },
    data () {
      return {
        user: {
          role: 'Student',
          username: 'michael23',
          email: '',
          firstName: '',
          lastName: '',
          address: '',
          city: '',
          country: '',
          phoneNumber: '',
          aboutMe: ``
        }
      }
    },
    methods: {
      notifyVue (verticalAlign, horizontalAlign) {

          this.$notifications.notify(
            {
              message: `<span><b> Success - </b> This form was updated ".alert-success"</span>`,
              icon: 'nc-icon nc-app',
              horizontalAlign: horizontalAlign,
              verticalAlign: verticalAlign,
              type: 4
            })
      },
      updateProfile () {
        alert('Your data: ' + JSON.stringify(this.user))
      },
      async createProfile () {
        try{
          const dataToSend = {
            id: this.user.id,
            nombre: this.user.firstName + ' ' + this.user.lastName,
            correo: this.user.email,
            telefono: this.user.phoneNumber,
            tipo: 'alumno',
          };

          const response = await axios.post('https://proyecto-sw2.up.railway.app/api/usuarios', dataToSend, {
            headers: {
              'Content-Type': 'application/json',
              // Otras cabeceras según sea necesario
            }
          });

          if(response.status === 200){
            console.log(response.data)
            this.$router.push({path: '/admin/user'})
            //this.notifyVue('top', 'center')

          }

        }catch (e) {
          console.log(e)
        }
      }
    }
  }

</script>
<style>

</style>
