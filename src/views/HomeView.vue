<script setup>
    import pitchDataRaw from "../data/pitches.json"
    import PitchCard from "../components/PitchCard"
    import { ref, watch } from "vue"
    import VueCookies from "vue-cookies"

    const pitchData = ref(pitchDataRaw)
    const oldSearch = VueCookies.get('oldSearch');
    const search = ref ( oldSearch ? oldSearch : "");

    const doSearch = () => {
        VueCookies.set('oldSearch', search.value, "3y");

        pitchData.value = Object.keys(pitchDataRaw)
            .filter( pitchImage => {
                const bool = pitchImage
                    .replace(/^(\w+\/)*/, "")
                    .replace(/.\w+$/, "")
                    .toLowerCase()
                    .includes(search.value.toLowerCase());
                return bool;
            })
            .reduce((obj, key) => {
                obj[key] = pitchDataRaw[key]
                return obj;
            }, {})
    };
    doSearch();


    watch(search, doSearch);
</script>


<template>
    <div class="search">
        <input id="GroundsSearch" v-model.trim="search" type="text" placeholder="Search..." />
    </div>
    <div class="grounds">
      <PitchCard 
        v-for="(key) in Object.keys(pitchData)" 
        :key="key"
        :image="key"
        :pitchList="pitchData[key]" />
    </div>
</template>

<style scoped>

input {
    min-width: 30rem;
    min-height: 3rem;
    font-size: 1.5rem;
    padding-left: 1rem;
    padding-right: 1rem;
}

</style>
