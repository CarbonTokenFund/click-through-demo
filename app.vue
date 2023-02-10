<template>

    <navbar v-model="search"/>

    <section class="container mx-auto">
        <h1 class="text-3xl mt-6">Fresh Produce &#x203a; Vegetables</h1>
        <infobar class="mt-4 mb-8">Wednesday orders open. Closes in 0h14m</infobar>
        <div class="flex flex-wrap">
            <a v-for="item in database.filter(item => search === '' || item.title.toLowerCase().indexOf(search.toLowerCase()) > -1)" href="#" class="mr-4 mb-8 flex bg-white border border-gray-200 rounded-xl shadow flex-row h-32">
                <img class="object-cover border border-red w-24 rounded-none rounded-l-lg" :src="`img/${item.url}`" alt="">
                <div class="w-12 flex flex-col justify-center items-center">
                    <div class="font-bold text-2xl text-green-500" @click="item.qty++">&#x25B2;</div>
                    <div class="font-medium text-3xl text-gray-800">{{item.qty}}</div>
                    <div class="font-bold text-2xl text-green-500" @click="item.qty > 0 ? item.qty-- : void(0)">&#x25BC;</div>
                </div>
                <div class="flex flex-col justify-between leading-normal w-48 py-1 relative">
                    <div class="flex">
                        <h5 class="flex text-xl font-bold tracking-tight text-gray-900 dark:text-white mt-2">{{ item.title }}</h5>
                        <h5 class="flex flex-grow justify-end text-xl font-bold tracking-tight text-red-700 dark:text-white mt-2 mr-2">R{{format(item.price)}}</h5>
                    </div>

                    <span class="text-sm italic text-gray-600">Packet 500g</span>
                    <span class="text-sm">Campbell Organics</span>
                    <span class="text-sm">Distance: <span class="font-bold">5km</span> Stock: <span class="font-bold">14</span></span>
                    <span class="absolute text-xs mr-1 px-2 py-0.5 bg-green-700 rounded-full text-white font-medium right-0 -mt-4 mr-6">Organic:PGS</span>
                </div>
            </a>
        </div>

        <h1 class="text-3xl mt-6">Fresh Produce &#x203a; Fruit</h1>
        <infobar class="mt-4 mb-8">Wednesday orders open. Closes in 0h14m</infobar>
        <h1 class="text-3xl mt-6">Locally made Edibles</h1>
        <h1 class="text-3xl mt-6">Shipped in Edibles</h1>

    </section>
    <div id="defaultModal" data-modal-backdrop="static" tabindex="-1" aria-hidden="true" class="fixed top-0 left-0 right-0 z-50 hidden w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-modal md:h-full bg-opacity-50 bg-black">
        <div class="relative w-full h-full max-w-2xl md:h-auto">
            <basket v-model="database"/>
        </div>
    </div>
</template>

<script setup>
import {onMounted, ref} from 'vue'
import {initModals} from 'flowbite'
import Navbar from "./components/navbar";
import Infobar from "./components/infobar";
import Basket from "./components/basket"

const search = ref('')
const database = ref([
    {
        qty: 0,
        title: 'Red Apple',
        description: 'Cripps Red',
        url: 'apple-red.jpg',
        price: 2.50
    }, {
        qty: 0,
        title: 'Avocado',
        description: 'Hass',
        url: 'avo.jpg',
        price: 4.50
    },
    {
        qty: 0,
        title: 'Baby Spinach',
        description: 'Fresh',
        url: 'baby-spinach.jpg',
        price: 1.50
    },
    {
        qty: 0,
        title: 'Broccoli',
        description: 'Fresh',
        url: 'broccoli.jpg',
        price: 8.00
    },
    {
        qty: 0,
        title: 'Carrots',
        description: 'Fresh',
        url: 'carrots.jpg',
        price: 3.00
    },
    {
        qty: 0,
        title: 'Cauliflower',
        description: 'Fresh',
        url: 'cauliflower-purple.jpg',
        price: 4.00
    },
    {
        qty: 0,
        title: 'Chili',
        description: 'Fresh',
        url: 'chili.jpg',
        price: 0.50
    },
    {
        qty: 0,
        title: 'Garlic',
        description: 'Fresh',
        url: 'garlic.jpg',
        price: 10.00
    },
    {
        qty: 0,
        title: 'Ginger',
        description: 'Fresh',
        url: 'ginger.jpg',
        price: 11.00
    },
    {
        qty: 0,
        title: 'Granadilla',
        description: 'Fresh',
        url: 'granadilla.jpg',
        price: 3.00
    }
])

let formatting_options = {
}

const randString = new Intl.NumberFormat("en-US", {minimumFractionDigits: 2});
const format = (value) => {
    return randString.format(value)
}

onMounted(() => {
    initModals();
})


</script>