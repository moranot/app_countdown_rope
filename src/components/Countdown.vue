<template>
    <div class="container" v-on:mouseover="hover = true;" v-on:mouseleave="hover = false">
        <div class="row">
            <div class="countdown-wrapper">
                <div id="countdown" :style="{ left: computedPositionLeft }"></div>
            </div>
            <label for="countdown">{{ countdown }}</label>
        </div>
        <div class="toolbar" :class="{ active: hover }">
            <button
                type="button"
                v-on:click="start"
                :disabled="active"
            >
                &#9654;
            </button>
            <button
                type="button"
                v-on:click="stop"
                :disabled="!active"
            >
                &#8214;
            </button>
            <button
                type="button"
                v-on:click="reset"
                :disabled="!active && this.id == 0"
            >
                &#8634;
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Countdown',
    data: function() {
        return {
            hover: false,
            max: 100,
            countdown: 100,
            increment: 10,
            id: 0,
            active: 0,
            positionLeft: 0,
            notifTag: 'Countdown App',
            notifTitle: 'Countdown Progress',
            notifBody: 'Finished!'
        }
    },
    methods: {
        start: function() {
            this.id = setInterval(this.count, 1000);
            this.active = 1;
        },
        count: function() {
            if (this.countdown <= 0 ) {
                clearInterval(this.id);
                this.notify();
                return;
            }
            this.countdown -= this.increment;
            this.updatePositionLeft();
        },
        notify: function() {
            const NOTIF_TITLE = this.notifTitle;
            const NOTIF_OPTIONS = {
                tag: this.notifTag,
                body: this.notifBody,
                silent: true
            };
            new Notification(NOTIF_TITLE, NOTIF_OPTIONS);
        },  
        stop: function() {
            clearInterval(this.id);
            this.active = 0;
        },
        reset: function() {
            clearInterval(this.id);
            this.active = 0;
            this.id = 0;
            this.positionLeft = 0;
            this.countdown = 100;
        },
        updatePositionLeft: function() {
            this.positionLeft = this.max - this.countdown;
        }
    },
    computed: {
        computedPositionLeft: function() {
            return 4 * this.positionLeft + 'px';
        }
    }
}
</script>

<style>
:root {
    --app-size: 30px;
}

.container, .row {
    display: flex;
}

.container {
    flex-direction: column;
    padding: 20px;
}

.row {
    width: 100%;
}

.countdown-wrapper {
    overflow: hidden;
    border-top-left-radius: var(--app-size);
    border-bottom-left-radius: var(--app-size);
}

#countdown {
    position: relative;
    background: url('../assets/rope.png');
    background-repeat: no-repeat;
    background-size: cover;
    height: var(--app-size);
    width: 400px;
    border-top-left-radius: var(--app-size);
    border-bottom-left-radius: var(--app-size);
    margin: calc(var(--app-size) / 10) 0;
}

label {
    color: #FFF;
    font-size: var(--app-size);
    text-align: center;
    width: 3rem;
    margin-left: 30px;
}

.toolbar {
    display: none;
}

.toolbar.active {
    display: block;
}

button {
    background-color: #FFF;
    height: var(--app-size);
    padding: 7px 15px;
    border: 1px solid #000;
    border-radius: 5px;
    margin-right: 15px;
}

button:not(:disabled):hover {
    color: #FFF;
    background-color: #000;
    border-color: #000;
}
</style>