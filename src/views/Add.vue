<template>
    <div>
    <div class="box ">
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
            }
        };
    },
    mounted (){
        this.newProduct = {}
    },
    methods: {
        add(){
            axios.post(`http://localhost:5000/api/product`, this.newProduct)
                .then(()=> {
                    this.$router.push({name: "Product"});
                })
                .catch((error) => {
                    console.log(error);
            });
        },
        cancelar(){
            this.$router.push({name: "Product"});
        }, 
    }
}
</script>

<style scoped>

</style>