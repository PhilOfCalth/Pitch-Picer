<script setup>
    import {defineProps} from "vue"
    import html2canvas from "html2canvas"

    const props = defineProps(['image', 'pitchList']);
    const groundsName = props.image.replace(/^(\w+\/)*/, "").replace(/.\w+$/, "");

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
        <h2>{{ groundsName }}</h2>
        <div class="ground">
            <img :alt="groundsName" :src="require('../assets/' + props.image)" />
            <div class="pitch"
                v-for="(pitch, index) in pitchList" 
                :key="props.image+index"
                :id="props.image+index"
                :style="'top: '+pitch.topLeftX+'px; left: '+pitch.topLeftY+'px; width: '+pitch.width+'px; height: '+pitch.height+'px;'"
                @click="printGrounds"
                ></div>
        </div>
</template>

<style scoped>
    .ground {
        position: relative;
        display: inline-block;
    }

    .pitch {
        position: absolute;
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