<template>
    <select v-on:change="onChange()" v-model="selected" class="form-select mt-10 block w-full border p-3 rounded">
        <option value="0">Select Country</option>
        <option v-for="country in countries.Countries" :value="country.ID" :key="country.ID">
            {{ country.Country }}
        </option>
    </select>
</template>

<script>

export default {
    name: 'CountrySelect',
    props: ['countries'],
    data() {
        return {
            selected: 0
        }
    },
    methods: {
        onChange() {
            const country = this.countries.Countries.find((item) => item.ID === this.selected)
            this.$emit('get-country', country)
        }
    },
    created () {
    },
}

</script>
