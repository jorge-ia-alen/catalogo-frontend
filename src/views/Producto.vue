<template>
  <div>
    <!--Cabecera-->
    <section class="hero is-small is-link">
      <div class="hero-body">
        <p class="title">
          Productos
        </p>
        <p class="subtitle">
          Aquí se muestra la lista de productos disponibles actualmente
        </p>
      </div>
    </section>

    <!--Búscador y filtros-->
    <div class="search-area">
      <nav class="level">
        <!-- Left side -->
        <form class="level-left" @submit.prevent="searchProducts">
          <div class="level-item">
            <div class="field has-addons">
              <p class="control is-expanded">
                <input class="input" type="text" placeholder="búsqueda de productos..." style="min-width: calc(45vw)"
                  v-model='buscador'>
              </p>
              <p class="control">
                <button class="button is-link">
                  <font-awesome-icon icon="search" />
                  Buscar
                </button>
              </p>
            </div>
          </div>
        </form>

        <div class="level-rigth">
          <div class="field has-addons">
            <p class="control">
              <button class="button is-link" @click.prevent='add'>
                Agregar
              </button>
            </p>
          </div>
        </div>
      </nav>
    </div>

    <!--Listado de productos-->
    <div class="list-catalogo">
      <div class="columns is-multiline">
        <div class="column is-one-quarter" v-for="product in productList" :key="product.id">
          <div class="container">

            <div class="card">

              <header class="card-header">
                <p class="card-header-title">
                  {{ product.nombre }}
                </p>
              </header>

              <div class="card-content">
                <div class="content">
                  <img src="../assets/images/generic-product.png" width="150" class="image-product"
                    :alt="product.nombre" />
                  <p v-if="product.descrip"><strong>Descripcion: </strong> {{ product.descrip }} </p>
                  <p v-if="product.estado"><strong>Estado: </strong> {{ product.estado }} </p>
                  <p v-if="product.precio"><strong>Precio: </strong> {{ product.precio }} </p>

                </div>


              </div>

              <footer class="card-footer">
                <nav class='level is-mobile'>
                  <div class="level-right">
                    <button class="button is-link" @click.prevent="edit(product)">
                      <font-awesome-icon icon="pen" />
                      Editar
                    </button>
                    <button class="button is-link" @click.prevent="del(product)">
                      <font-awesome-icon icon="trash" />
                      Borrar
                    </button>
                  </div>
                </nav>
              </footer>

            </div>

          </div>
        </div>
      </div>
    </div>

    <div class="pagination-area">
      <PaginationComponent :page="pagination.page" :pages="pagination.pages" @selected="(page) => searchProducts(page)">
      </PaginationComponent>
    </div>
    <div class="pageloader is-bottom-to-top is-link" style="opacity: 0.8" :class="{'is-active': loading}"><span
        class="title">Cargando productos...</span></div>

  </div>
</template>
  
<script>
import { HTTP } from '@/http'
import axios from 'axios';
import PaginationComponent from '../components/PaginationComponent';

export default {
  name: "Producto",
  components: { PaginationComponent },
  data() {
    return {
      loading: false,
      search: {
      },
      buscador: null,
      pagination: {
        page: 1,
        pages: 1
      },
      productList: [],
    };
  },
  mounted() {
    this.searchProducts();
  },
  methods: {
    add() {
      this.$router.push({ name: "addProduct" });
    },
    edit(instance) {
      this.$router.push({ name: "editProduct", query: { inst: instance } });
    },
    del(instance) {
      axios.delete(`http://localhost:5000/api/productos/${instance.id}`)
        .then(() => {
          this.searchProducts();
        })
        .catch((error) => {
          console.log(error);
          this.searchProducts();
        });
    },
    async searchProducts(page) {
      this.loading = true;
      this.search.nombre = this.buscador
      try {
        let data = (await HTTP.post(`/search/productos`, this.search, {
          params: {
            page: page ? page : 1
          }
        })).data;
        if (data.items) {
          this.productList = data.items;
          this.pagination.page = data.page;
          this.pagination.pages = data.pages;
        } else {
          this.productList = data;
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
.list-catalogo {
  margin: 50px;
}

.image-product {
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 10px;
  width: 50%;
}

.search-area {
  margin-top: 20px;
  margin-left: 50px;
  margin-right: 50px;
}

.pagination-area {
  margin: 3%;
}
</style> 