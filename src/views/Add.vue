<template>
    <div>
    <div class="box ">
    <div class="field">
    <label class="label">Nombre</label>
    <div class="control">
        <input class="input" type="text" v-model="newProduct.nombre" placeholder="Nombre del producto" required>
    </div>
    </div>

    <div class="field">
    <label class="label">Descripcion</label>
    <div class="control">
        <input class="input" type="text" v-model="newProduct.descrip" placeholder="Descripcion del producto" required>
    </div>
    </div>

    <div class="field">
    <label class="label">Estado</label>
    <div class="control">
        <input class="input" type="text" v-model="newProduct.estado" placeholder="Estado del producto" required>
    </div>
    </div>

    <div class="field">
    <label class="label">Precio</label>
    <div class="control">
        <input class="input" type="text" v-model="newProduct.precio" placeholder="Estado del producto" required>
    </div>
    </div>

    <div class="field">
    <label class="label">Categoria</label>
    <div class="control">
        <input class="input" type="text" v-model="newProduct.categoria_id" placeholder="Categoria del producto" required>
    </div>
    </div>

    <div class="field">
    <label class="label">Proveedor</label>
    <div class="control">
        <input class="input" type="text" v-model="newProduct.proveedor_id" placeholder="Proveedor del producto" required>
    </div>
    </div>

    <div class="field is-grouped">
    <div class="control">
        <button class="button is-link" id="submitButton" @click.prevent="add">Guardar</button>
    </div>
    <div class="control">
        <button class="button is-text" @click.prevent="cancelar">Cancelar</button>
    </div>
    </div>
    </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name:"AddProduct",
    data(){
        return {
            newProduct: {
                descrip: null, 
                estado: null,
                precio: null,
                proveedor_id: null, 
                categoria_id: null,
            }
        };
    },
    mounted (){
        this.newProduct = {}
    },
    methods: {
        add(){
            axios.post(`http://localhost:5000/api/productos`, this.newProduct)
                .then(()=> {
                    this.$router.push({name: "producto"});
                })
                .catch((error) => {
                    console.log(error);
            });
        },
        cancelar(){
            this.$router.push({name: "producto"});
        }, 
    }
}
</script>

<style scoped>

</style>