<script setup>
    import {defineProps} from "vue"
    import html2canvas from "html2canvas"

    const props = defineProps(['image', 'pitchList']);
    let groundsName = props.image
                            .replace(/^(\w+\/)*/, "")
                            .replace(/.\w+$/, "");

    const midCapitalRegex = /([a-z])([A-Z])/g;

    if(groundsName.match(midCapitalRegex)){
      groundsName = groundsName.replaceAll(midCapitalRegex, "$1 $2");
    }

    const printGrounds = async (event) => {
        const toPrintDiv = event.target.parentNode;
        event.target.className = 'selected'
        
        const printCanvas = await html2canvas(toPrintDiv, { type: "dataURL" });
        const date = (new Date()).toLocaleDateString('en-GB');

        const link = document.createElement("a");
        link.setAttribute("download", "highlited_pitch_"+date+".png");
        link.setAttribute(
            "href",
            printCanvas
            .toDataURL("image/png")
            .replace("image/png", "image/octet-stream")
        );
        link.click();

        event.target.className = 'pitch'
    }

</script>

<template>
    <div class="ground-card" >
        <h2>{{ groundsName }}</h2>
        <div class="ground">
            <img :alt="groundsName" :src="require('../assets/' + props.image)" />
            <div class="pitch"
                v-for="(pitch, index) in pitchList" 
                :key="props.image+index"
                :id="props.image+index"
                :style="'left: '+pitch.topLeftX+'px; top: '+pitch.topLeftY+'px; width: '+pitch.width+'px; height: '+pitch.height+'px; transform: rotate('+pitch.rotate+'deg);'"
                @click="printGrounds"
                ></div>
        </div>
    </div>
</template>

<style scoped>
    .ground-card{
        margin-top: 3rem;
        border-style: dashed;
        border-width: 12px;
        border-color: rgb(200, 200, 200);
        border-radius: 50px;
        display: inline-block;
    }

    .ground {
        position: relative;
        display: inline-block;
    }

    h2 {
        font-size: 2rem;
    }

    .pitch {
        position: absolute;
        transform: rotate(20deg);
        z-index: 10;
        border-color: yellow;
        border-width: 3px;
        border-style: dashed;
    }

    .pitch:hover {
        cursor: pointer;
        background-color: rgba(144, 108, 186, 0.6);
        border-color: red;
    }

    .selected {
        position: absolute;
        z-index: 10;
        border-width: 3px;
        border-style: dashed;
        cursor: pointer;
        background-color: rgba(144, 108, 186, 0.6);
        border-color: red;
    }
</style>