<template>
    <!--Modal Overlay-->
    <div v-show = "modalActive" style = "position:absolute; width: 100%; background-color: rgba(0,0,0, 0.5); height:100vh; top:0; left: 0; display:flex; justify-content: center; padding-top: 100px; z-index: 1000;overflow: hidden; position: fixed;">
        <!--Modal-->
        <div v-if = "modalActive" style = "padding: 200px; background-color: white; align-self: flex-start; margin-top: 50px; border-radius: 20px;">
            <button @click = "$emit('close-modal')" style = "float:right; border:none; color:#666; padding:20px 25px 20px 25px; font-size: medium; border-radius: 10px; cursor:pointer;">Close</button>
            <slot />

        </div>
    </div>
</template>

<script setup>

    defineEmits(["close-modal"]);

    defineProps({
        modalActive: {
            type: Boolean,
            default: false,
        },
    });

</script>

<style lang="css" scoped>

</style>