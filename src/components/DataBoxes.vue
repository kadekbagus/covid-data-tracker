<template>
    <div class="grid md:grid-cols-2 gap-4 mt-10">
        <div class="shadow-md bg-blue-100 p-5 text-center rounded">
            <h3 class="text-xl text-blue-900 font-bold mb-4">Recovery Rate</h3>
            <div class="text-4xl mb-4">
                <span class="font-bold text-green-500">{{ recoveryRate }}%</span>
            </div>
            <span class="text-blue-900 font-bold">of total cases</span>
        </div>

        <div class="shadow-md bg-blue-100 p-5 text-center rounded">
            <h3 class="text-xl text-blue-900 font-bold mb-4">Death Rate</h3>
            <div class="text-4xl mb-4">
                <span class="font-bold text-gray-500">{{ deathRate }}%</span>
            </div>
            <span class="text-blue-900 font-bold">of total cases</span>
        </div>
    </div>
</template>


<script>


export default {
    name: 'DataBoxes',
    props: ['deathRate', 'recoveryRate'],
    methods: {
    }
}
</script>
