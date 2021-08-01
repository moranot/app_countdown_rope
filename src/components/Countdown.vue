<template>
    <div class="container" v-on:mouseover="hover = true;" v-on:mouseleave="hover = false">
        <div class="row">
            <div class="countdown-wrapper">
                <div id="countdown" :style="{ left: computedPositionLeft, transition: 'left 1s linear' }"></div>
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
                :disabled="!active || finished"
            >
                &#8214;
            </button>
            <button
                type="button"
                v-on:click="reset"
                :disabled="!active && this.countdownId == 0"
            >
                &#8634;
            </button>
        </div>
    </div>
</template>

<script>
import sound from '../assets/sound/rope.mp3';
import { Howl } from 'howler';

export default {
    name: 'Countdown',
    data: function() {
        return {
            hover: true,
            max: 100,
            countdown: 100,
            increment: 10,
            countdownId: 0,
            positionId: 0,
            active: 0,
            finished: false,
            positionLeft: 0,
            notifTag: 'Countdown App',
            notifTitle: 'Countdown Progress',
            notifBody: 'Finished!',
            sound: new Howl({
                src: [sound],
                volume: 0.25
            })
        }
    },
    methods: {
        start: function() {
            this.countdownId = setInterval(this.count, 1000);
            this.sound.play();
            this.active = 1;
            this.finished = false;
        },
        count: function() {
            this.countdown -= this.increment;
            this.updatePositionLeft();
            if (this.countdown <= 0) {
                clearInterval(this.countdownId);
                this.finished = true;
                this.sound.stop();
                this.notify();
                return;
            }
        },
        notify: function() {
            new Notification(this.notifTitle, {
                tag: this.notifTag,
                body: this.notifBody,
                silent: true
            });
        },  
        stop: function() {
            clearInterval(this.countdownId);
            this.active = 0;
            this.sound.stop();
        },
        reset: function() {
            this.stop()
            this.countdownId = 0;
            this.positionLeft = 0;
            this.countdown = 100;
        },
        updatePositionLeft: function() {
            this.positionLeft = this.max - this.countdown;
        },
    },
    computed: {
        computedPositionLeft: function() {
            return (4 * this.positionLeft) + 'px';
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
    align-items: center;
}

.countdown-wrapper {
    display: inline-block;
    overflow: hidden;
    border-top-left-radius: var(--app-size);
    border-bottom-left-radius: var(--app-size);
}

#countdown {
    position: relative;
    background: url('../assets/bar/rope.png');
    background-repeat: no-repeat;
    background-size: cover;
    height: var(--app-size);
    width: 400px;
    border-top-left-radius: var(--app-size);
    border-bottom-left-radius: var(--app-size);
}

label {
    display: inline-block;
    color: #FFF;
    text-shadow: 2px 2px 3px #000;
    font-size: var(--app-size);
    text-align: center;
    min-width: 8rem;
    padding: 7px 30px;
    background: linear-gradient(350deg, hsl(32deg, 15%, 55%), hsl(32deg, 15%, 40%));
    border: 1px solid hsl(32deg, 15%, 21%);
    border-radius: 25%;
    box-shadow: inset 1px 2px 6px 4px hsl(32deg 15% 31%),
                inset 0px 1px 0px 3px hsl(32deg 4% 30%),
                inset 1px 1px 2px 4px hsl(32deg 72% 83%);
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