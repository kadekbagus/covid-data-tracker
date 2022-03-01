<template>
    <div class="container mx-auto">
    <div class="grid mb-10">
         <h1 class="text-5xl text-blue-900 font-bold mb-4">{{ countryName }}</h1>
         <p>Last update <span class="font-bold">{{ lastUpdated }}</span></p>
    </div>
    <div class="grid md:grid-cols-3 gap-5 shadow-md bg-blue-100 p-5 text-center rounded">
        <div>
            <h3 class="text-xl text-blue-900 font-bold mb-4">Total Confirmed</h3>
            <div class="text-3xl mb-4">
                <span class="font-bold text-red-500">{{ formatNumber(totalConfirmed) }}</span>
            </div>
        </div>
        <div>
            <h3 class="text-xl text-blue-900 font-bold mb-4">Total Recovered</h3>
            <div class="text-3xl mb-4">
                <span class="font-bold text-green-500">{{ formatNumber(totalRecovered) }}</span>
            </div>
        </div>
        <div>
            <h3 class="text-xl text-blue-900 font-bold mb-4">Total Deaths</h3>
            <div class="text-3xl mb-4">
                <span class="font-bold text-gray-500">{{ formatNumber(totalDeaths) }}</span>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
export default {
    name: 'InfoBox',
    props: ['totalConfirmed', 'totalRecovered', 'totalDeaths', 'lastUpdated', 'countryName'],
    methods: {
        formatNumber(number) {
            return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
        }
    }
}
</script>
