<template>
  <v-toolbar color="blue">
      <v-toolbar-title>Productos</v-toolbar-title>

      <v-spacer></v-spacer>
      <p>Hola, {{this.admin.name}}</p>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>

      <v-btn @click="logout">Salir</v-btn>
  </v-toolbar>
  <v-alert
      v-model="alert"
      color="orange"
      closable
      type="warning"
      class="mb-3"
      :text="alertText"></v-alert>
  <v-row class="mx-auto my-3">
    <v-col sm="1" md="2" lg="3"></v-col>
    <v-col cols="12" sm="10" md="8" lg="6">
      <v-card>
        <v-tabs
          v-model="tab"
          bg-color="secondary"
        >
            <v-tab value="register">Nuevo artículo</v-tab>
            <v-tab value="add">Agregar unidades</v-tab>
        </v-tabs>
        <v-card-text>
          <v-window v-model="tab">
            <v-window-item value="register">
              <v-form>
                <v-text-field
                  v-model="code"
                  label="Código"
                ></v-text-field>
                <v-text-field
                  v-model="article"
                  label="Artículo"
                ></v-text-field>
                <v-text-field
                  v-model="price"
                  type="number"
                  label="Precio"
                ></v-text-field>
                <v-btn @click="register" block color="secondary" class="mt-2 btn">Registrar producto</v-btn>
              </v-form>
            </v-window-item>
            <v-window-item value="add">
              <v-form>
                <v-text-field
                  v-model="addCode"
                  label="Código"
                ></v-text-field>
                <v-text-field
                  v-model="units"
                  label="Unidades"
                ></v-text-field>
                <v-btn @click="addUnits" block color="secondary" class="mt-2 btn">Añadir</v-btn>
              </v-form>
            </v-window-item>
          </v-window>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
  <v-table class="w-75 mx-auto mb-4">
    <template v-slot:default>
      <thead>
        <tr>
          <th></th>
          <th class="text-left">
            Código
          </th>
          <th class="text-left">
            Artículo
          </th>
          <th class="text-left">
            Precio
          </th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(article,index) in articles" :key="index"
        >
          <td><v-badge :content="article.units" inline></v-badge></td>
          <td><input style="width:100px" type="text" :value=" article.code"/></td>
          <td><input style="width:300px" type="text" :value=" article.name"/></td>
          <td><span :id="'span-'+index" :data-id="index" @click="showInputPrice">{{formatter.format(article.price)}}</span><input @blur="hideInputPrecio" :id="'price-'+index" :data-id="index" type="number" style="width:100px;display:none" :value="article.price"/></td>
          <td>
            <v-btn color="red" @click="remove(index)" icon><v-icon>mdi-delete</v-icon></v-btn></td>
        </tr>
      </tbody>
    </template>
  </v-table>
</template>

<script>
import { defineComponent } from 'vue';



export default defineComponent({
  name: 'ListView',
  data: () => ({
    code: '',
    article: '',
    price: '',
    addCode: '',
    units: 0,
    formatter: new Intl.NumberFormat('sp-SP', { style: 'currency', currency: 'EUR' }),
    alert: false,
    alertText: "",
    tab:"register"
  }),
  methods:{
    checkAuth: function() {
     if(!this.admin.auth){
      this.$router.push('/login');
     }
    },
    logout: function(){
      this.admin.auth = false;
      this.$router.push('/login');
    },
    register: function(){
      if(this.code && this.article && this.price){
        this.articles.push({code:this.code,name:this.article,price:this.price,units:1});
        this.code = this.article = this.price ="";
        this.alert = false;
      }else{
        this.alertText = "Faltan campos";
        this.alert = true;
      }
    },
    remove: function(index){
      if(confirm("¿Borrar el artículo?")){
        this.articles.splice(index,1);
      }
    },
    showInputPrice: function(event){
      event.target.style.display = 'none';
      let input = document.getElementById('price-'+event.target.dataset.id);
      input.style.display = 'block';
      input.focus();
    },
    hideInputPrecio: function(event){
      event.target.style.display = 'none';
      let span = document.getElementById('span-'+event.target.dataset.id);
      span.style.display = 'inline';
      this.articles[event.target.dataset.id].price = event.target.value;
    },
    addUnits: function(event){
      let found = false;
      for(let key in this.articles){
        if(this.articles[key].code == this.addCode){
          
          this.articles[key].units += parseInt(this.units);

          this.addCode = "";
          this.units = 0;
          found = true;
          break;
        }
      }
      if(!found){
        this.alert = true;
        this.alertText = "Artículo no encontrado";
      }
    }
  },
  beforeMount(){
    this.checkAuth()
  },
  props:{
    admin:{
      type: Object
    },
    articles:{
      type: Array
    }
  }
});
</script>
