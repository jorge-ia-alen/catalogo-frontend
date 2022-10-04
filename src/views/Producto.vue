<template>
    <div>
      <!--Cabecera-->
      <section class="hero is-small is-link">
        <div class="hero-body">
          <p class="title">
            Productos
          </p>
          <p class="subtitle">
            Aquí se muestra la lista de productos
          </p>
          </div>
      </section>  
      <!--Búscador y filtros-->
      <div class="search-area">
        <nav class="level">
          <!-- Left side -->
          <form class="level-left" @submit.prevent="searchProduct">
            <div class="level-item">
              <div class="field has-addons">
                <p class="control is-expanded">
                  <input class="input" type="text" placeholder="búsqueda de productos..." style="min-width: calc(45vw)" v-model='buscador'>
                </p>
                <p class="control">
                  <button class="button is-link">
                    <font-awesome-icon icon="search"/>
                    Buscar
                  </button>
                </p>
              </div>
            </div>
          </form>
          <div class="level-rigth">
            <div class="field has-addons">
              <p class="control">
                <button class = "button is-link" @click.prevent='add'>
                  Agregar
                </button>
              </p>
            </div>
          </div>
        </nav>
      </div>
  
      <!--Listado de productos-->
      <div class="list-product">
        <div class="columns is-multiline">
          <div class="column is-one-quarter" v-for="producto in productList" :key="producto.id">
            <div class="container">
  
              <div class="card">
  
                <header class="card-header">
                  <p class="card-header-title">
                    {{ producto.id }}
                  </p>
                </header>
  
                <div class="card-content">
                  <div class="content">
                    <p v-if="producto.descrip"><strong>Descripcion: </strong> {{ producto.descrip }} </p>
                    <p v-if="producto.status"><strong>Estado: </strong> {{ producto.status }} </p>
                  </div>
                  <nav class='level is-mobile'>
                    <div class="level.right">
                      <button class="button is-link" @click.prevent='edit(producto)'>
                        Editar                    
                      </button>   
                      <button class="button is-link" @click.prevent='del(producto)'>
                        Borrar                    
                      </button>                    
                    </div>
                  </nav>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div> 
    </div>     
  </template>
  
  <script>
  import {HTTP} from '@/http'
  import axios from 'axios';
  export default { // eslint-disable-next-line
      name: 'Producto',
      data() {
          return {
              productList: [],
              loading: false,
              search: {
              },
              buscador: null, 
              pagination:{
                  page: 1,
                  pages: 1
              },
          };
      },
      mounted (){
          this.searchProduct();
      },
      methods: {
        add(){
          this.$router.push({name: 'addProduct'})
        },
        edit(instance) {
            this.$router.push({name: 'editProduct', query:{inst:instance}})
          }, 
          del(instance) {
            axios.delete(`http://localhost:5000/api/tasks/${instance.id}`)
              .then(()=> {
                this.searchProduct();
              }
              )
              .catch((error) => {
                console.log(error);
                this.searchProduct();
              }
              )
          }, 
          async searchProduct(page){
                  this.loading = true;
                  this.search.id =this.buscador 
              try {
              let data = (await HTTP.post(`/search/producto`, this.search, {
              params: {
              page: page ? page : 1
              }
              })).data;
              if (data.items) {
              this.taskList = data.items;
              this.pagination.page = data.page;
              this.pagination.pages = data.pages;
              } else {
              this.taskList = data;
              this.pagination = {};
              }
          } catch (error) {
              //eslint-disable-next-line no-prototype-builtins
              if (!error.hasOwnProperty('response')) {
              this.notificar('error', this.title, 'Ha ocurrido un error inesperado. Error: ' + error);
              }
              if (error.response.status !== 404) {
              this.notificar('error', this.title, 'No se han podido obtener la lista de proyectos. Error: ' + error);
              }
              this.productList = [];
          } finally {
              this.loading = false;
          }
          }
          }
          }
  </script>
  
  <style scoped>
  
  </style> 