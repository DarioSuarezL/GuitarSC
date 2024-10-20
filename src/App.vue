<script setup>
import {ref, onMounted, watch} from 'vue'
import {db} from './data/guitars'

import Guitar from './components/Guitar.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'


const guitars = ref([])
const cart = ref([])
const guitar = ref({})

const saveLocalStorage = () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
}

watch(cart, saveLocalStorage, {deep: true})

onMounted(() => {
    guitars.value = db
    guitar.value = db[3] //Guitarra por defecto, mala practica

    const localCart = localStorage.getItem('cart')
    if (localCart) {
        cart.value = JSON.parse(localCart)
    }
})


const addToCart = (guitar) => {
    const exists = cart.value.findIndex(item => item.id === guitar.id)

    if (exists !== -1) {
        cart.value[exists].quantity++
    }else{
        guitar.quantity = 1
        cart.value.push(guitar)
    }
}

const removeFromCart = (id) => {
    cart.value = cart.value.filter(item => item.id !== id)
}

const clearCart = () => {
    cart.value = []
}

const addQuantity = (id) => {
    const exists = cart.value.findIndex(item => item.id === id)
    
    if (exists === -1) {
        return
    }

    cart.value[exists].quantity++

}

const subtractQuantity = (id) => {

    const exists = cart.value.findIndex(item => item.id === id)

    if (exists === -1) { //Logicamente nunca sucederá, pero mejor prevenir
        return
    }

    if (cart.value[exists].quantity === 1) {
        removeFromCart(id)
        return
    }

    cart.value[exists].quantity--
}


</script>

<template>

    <Header 
        :cart="cart"
        :guitar="guitar"
        @add-quantity="addQuantity"
        @subtract-quantity="subtractQuantity"
        @remove-from-cart="removeFromCart"
        @clear-cart="clearCart"
        @add-to-cart="addToCart"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitar 
                v-for="guitar in guitars"
                :guitar="guitar"
                @add-to-cart="addToCart"
            />
            <!-- <p v-for="guitar in guitars">{{ guitar.nombre }}</p> -->
        </div>
    </main>


    <Footer />

</template>

<style scoped>

</style>
