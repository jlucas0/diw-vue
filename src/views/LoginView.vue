<template>
  <v-sheet width="300" class="mx-auto mt-5">
    <v-alert
      v-model="alert"
      color="orange"
      closable
      type="warning"
      class="mb-3"
      text="Credenciales incorrectas"></v-alert>
    <v-form>
      <v-text-field
        v-model="user"
        label="Usuario"
      ></v-text-field>
      <v-text-field
        v-model="password"
        type="password"
        label="Clave"
      ></v-text-field>
      <v-btn @click="acceder" block color="primary" class="mt-2 btn">Acceder</v-btn>
    </v-form>
  </v-sheet>
</template>

<script>
import { defineComponent } from 'vue';



export default defineComponent({
  name: 'LoginView',
  data: () => ({
      user: '',
      password: '',
      alert: false,
    }),
  methods:{
      acceder: function(event){
        if(this.user == this.admin.name && this.password == this.admin.password){
          this.admin.auth = true;
          this.$router.push('/');
        }else{
          this.alert = true;
        }
      }
    },
  props:{
    admin:{
      type: Object
    }
  }
});
</script>
