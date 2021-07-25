<template>
    <Countdown></Countdown>
</template>

<script>
import { defineComponent } from 'vue'
import Countdown from './components/Countdown.vue'

export default defineComponent({
    name: 'App',
    components: {
       Countdown
    }
});
</script>

<style>
* {
    box-sizing: border-box;
}
body {
    font-family: 'Arial';
    overflow: hidden;
}

#window {
    -webkit-app-region: drag;
    display: block;
    width: 100vw;
    height: 100vh;
}

#app {
    position: relative;
    z-index: 1;
}

button {
    -webkit-app-region: no-drag;
}
</style>
