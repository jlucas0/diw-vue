<template>
  <router-view :admin="admin" :articles="articles"></router-view>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
    admin: {
      name:'admin',
      password: '1234',
      auth: false
    },
    articles: [
      {code:"v82di",name:"Desconfigurador avanzado",price:8499.99,units:1},
      {code:"i39kf",name:"Nebulizador ultrasónico",price:3280,units:1},
      {code:"o45o2",name:"Iterador metafórico",price:5321.60,units:1}
    ]
  })
}
</script>
