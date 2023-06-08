<template>
  <div class="app-container">
     <h1 class="app-title">Bienvenido a mi aplicación Nuxt</h1>
 
     <!-- Otros contenidos -->
 
     <!-- Enlace a la página de registro -->
     <nuxt-link class="register-link" to="/register1">Ir a la página de Registro</nuxt-link>
   </div>
 </template>
 
 <script>
 export default {
   name: 'IndexPage'
 }
 </script>
 
 <style scoped>
 .app-container {
   width: 100%;
   max-width: 1200px;
   margin: 0 auto;
   padding: 20px;
   font-family: Arial, sans-serif;
   color: #333;
   box-sizing: border-box;
 }
 
 .app-title {
   color: #2d3748;
   font-size: 2.5rem;
   margin-bottom: 1rem;
   text-align: center;
 }
 
 .register-link {
   display: inline-block;
   margin-top: 1rem;
   padding: 10px 20px;
   color: #fff;
   background-color: #2d3748;
   border-radius: 4px;
   text-decoration: none;
   transition: background-color 0.3s ease;
 }
 
 .register-link:hover {
   background-color: #4a5568;
 }
 </style>
 
