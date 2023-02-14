<template>
    <!-- STEP 1 -->
    <div  v-if="step === 1" class="relative bg-white rounded-lg shadow">
        <!-- Modal header -->
        <div class="flex items-start justify-between p-4 border-b rounded-t">
            <h3 class="text-xl font-semibold text-gray-900">
                Your basket
            </h3>
            <button @click="checkoutClose" type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center">
                <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                <span class="sr-only">Close modal</span>
            </button>
        </div>

        <!-- Modal body -->
        <div class="p-6 space-y-6">
            <div class="relative overflow-x-auto">
                <table class="w-full text-sm text-left text-gray-500">
                    <thead class="text-xs text-gray-700 uppercase bg-gray-50">
                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Quantity
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Product Name
                        </th>
                        <th scope="col" class="px-6 py-3 text-right">
                            Each
                        </th>
                        <th scope="col" class="px-6 py-3 text-right">
                            Cost
                        </th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="item in database.filter(item => item.qty > 0)" class="bg-white border-b">
                        <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
                            {{item.qty}}
                        </td>
                        <td class="px-6 py-4">
                            {{item.title}}
                        </td>
                        <td class="px-6 py-4 text-right">
                            R{{format(item.price)}}
                        </td>
                        <td class="px-6 py-4 text-right">
                            R{{format(item.qty * item.price)}}
                        </td>
                    </tr>
                    <tr>
                        <td class="px-6 py-4">1</td>
                        <td class="px-6 py-4">Delivery (Anywhere in South Africa)</td>
                        <td class="px-6 py-4 text-right">R99.00</td>
                        <td class="px-6 py-4 text-right">R99.00</td>
                    </tr>
                    <tr>
                        <td class="px-6 py-4"><span v-if="voluntary > 0"> {{voluntary}}%</span><span v-if="voluntary < 5">Free</span></td>
                        <td class="px-6 py-4"><span class="font-medium">CarbonToken&trade;</span> investment</td>
                        <td class="px-6 py-4">$CTF{{formatToken(((voluntary / 100) * (total + 99) / 1000) + (total + 99) / 100000)}}</td>
                        <td class="px-6 py-4 text-right">R{{format((voluntary * (total + 99) / 100)) }}</td>
                    </tr>
                    <tr>
                        <td class="px-6 py-4"></td>
                        <td class="px-6 py-4 font-bold">You Pay</td>
                        <td class="px-6 py-4"></td>
                        <td class="px-6 py-4 text-right font-bold">R{{format(total + 99 + (voluntary * (total + 99) / 100))}}</td>
                    </tr>
                    <tr>
                        <td colspan="4" class="px-6 pt-10">
                            <div class="flex flex-row">
                                <label for="default-range" class="block mb-2 text-sm font-thin text-gray-900 inline"><span class="font-medium">CarbonToken&trade;</span> investment ({{voluntary}}%)</label>
                                <svg aria-hidden="true" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" class="ml-2 w-6 h-6 cursor-pointer ">
                                    <path d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" stroke-linecap="round" stroke-linejoin="round"></path>
                                </svg>
                            </div>
                            <input v-model="voluntary" id="default-range" type="range" min="0" max="10" step="1" class="text-blue-600 w-full h-2 bg-blue-300 shadow-inner rounded-lg appearance-none cursor-pointer">
                            <!--
                            <div role="tooltip" style="z-index: 10000000" class=" bg-red-500 absolute inline-block px-3 py-2 text-sm font-medium text-white bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700">
                                Tooltip content
                                <div class="tooltip-arrow" data-popper-arrow></div>
                            </div>
                            -->
                        </td>
                    </tr>
                    </tbody>
                </table>

            </div>
        </div>

        <!-- Modal footer -->
        <div class="flex items-center p-6 space-x-2 border-t border-gray-200 rounded-b">
            <button type="button" @click="checkoutClose" class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10">Continue shopping</button>
            <button type="button" @click="step++" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">Checkout</button>
        </div>
    </div>


    <!-- STEP 2 -->
    <div  v-if="step === 2" class="relative bg-white rounded-lg shadow">
        <!-- Modal header -->
        <div class="flex items-start justify-between p-4 border-b rounded-t">
            <h3 class="text-xl font-semibold text-gray-900">
                Payment details
            </h3>
            <button type="button" @click="checkoutClose" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center">
                <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                <span class="sr-only">Close modal</span>
            </button>
        </div>

        <!-- Modal body -->
        <div class="p-6 space-y-6">
            <div class="flex flex-col w-64">
                <input class="my-1 shadow-inner bg-gray-50 rounded rounded-xl border border-gray-200 py-1.5 px-2 outline-0 font-medium text-sm" placeholder="name on card">
                <input class="my-1 shadow-inner bg-gray-50 rounded rounded-xl border border-gray-200 py-1.5 px-2 outline-0 font-medium text-sm" placeholder="card number">
                <div class="flex flex-row">
                    <input class="my-1 w-32 mr-8 shadow-inner bg-gray-50 rounded rounded-xl border border-gray-200 py-1.5 px-2 outline-0 font-medium text-sm" placeholder="exp date">
                    <input class="my-1 w-24 shadow-inner bg-gray-50 rounded rounded-xl border border-gray-200 py-1.5 px-2 outline-0 font-medium text-sm" placeholder="CCCV">
                </div>
            </div>
        </div>

        <!-- Modal footer -->
        <div class="flex items-center p-6 space-x-2 border-t border-gray-200 rounded-b">
            <button type="button" @click="step++" class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10">Previous</button>
            <button type="button" @click="step++" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">Pay</button>
        </div>

    </div>

    <!-- STEP 3 -->
    <div  v-if="step === 3" class="relative bg-white rounded-lg shadow">
        <!-- Modal body -->
        <div class="p-6 space-y-6 flex flex-col items-center justify-center py-20">
            <svg aria-hidden="true" class="w-32 h-32 mr-2 text-gray-200 animate-spin fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
                <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
            </svg>
            <span class="sr-only">Loading...</span>
            <div class="text-gray-600 text-xl">Processing your payment</div>
        </div>
    </div>

    <!-- STEP 4 -->
    <div  v-if="step === 4" class="relative bg-white rounded-lg shadow">
        <!-- Modal header -->
        <div class="flex items-start justify-between p-4 border-b rounded-t">
            <h3 class="text-xl font-semibold text-gray-900">
                Thank you
            </h3>
            <button type="button" @click="checkoutClose" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center">
                <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                <span class="sr-only">Close modal</span>
            </button>
        </div>

        <!-- Modal body -->
        <div class="p-6 space-y-6">
            <div class="relative overflow-x-auto">
                <div class="text-2xl">Thank you!</div>
                <div class="mt-2 text-xl">We have received your payment and your order has been placed.</div>
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-16 h-16 my-4 text-green-600">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12.75l6 6 9-13.5" />
                </svg>
                <div class="text-2xl">Congratulations!</div>
                <div class="mt-2 text-xl">[Logo Animation]</div>
                <div class="block mt-6 font-thin text-gray-900">You earned <span class="font-bold">$CTF{{formatToken((total + 99) / 100000)}}</span> for your purchase.</div>
                <div class="block mt-2 font-thin text-gray-900">In addition your purchased <span class="font-bold">$CTF{{formatToken((voluntary / 100) * (total + 99) / 1000)}}</span></div>

            </div>
        </div>

        <!-- Modal footer -->
        <div class="flex items-center p-6 space-x-2 border-t border-gray-200 rounded-b">
            <button @click="checkoutClose" type="button" class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10">Close</button>
        </div>

    </div>
</template>
<script setup>

import {computed, ref, watchEffect} from "vue";
import { Tooltip } from 'flowbite-vue'

const step = ref(1);
const voluntary = ref(0);

const emit = defineEmits(['update:modelValue', 'checkoutClose'])

const checkoutClose = () => {
    emit('checkoutClose')
}

const props = defineProps({
    modelValue: {
        type: [Object],
        default: ''
    }
})

watchEffect(() => {
    if (step.value === 3) {
        setTimeout(() => {step.value++}, 2000)
    }
})

const database = computed({
    get() {
        return props.modelValue
    },

    set(value) {
        return emit('update:modelValue', value)
    }
})

const total = computed(() => {
    return database.value.reduce((accumulator, current) => accumulator + (current.qty * current.price), 0);
})

//@TODO: Repeated
const randString = new Intl.NumberFormat("en-US", {minimumFractionDigits: 2, maximumFractionDigits:2});
const format = (value) => {
    return randString.format(value)
}

const tokenString = new Intl.NumberFormat("en-US", {minimumFractionDigits: 4, maximumFractionDigits:4});
const formatToken = (value) => {
    return tokenString.format(value)
}


</script>