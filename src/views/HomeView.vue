<script setup>
    import pitchData from "../data/pitches.json"
    import PitchCard from "../components/PitchCard"

</script>


<template>
    <div class="search">
    </div>
    <div class="grounds">
      <PitchCard 
        v-for="(key) in Object.keys(pitchData)" 
        :key="key"
        :image="key"
        :pitchList="pitchData[key]" />
    </div>
</template>