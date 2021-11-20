<template>
    <GameFinished 
        v-if="isGameFinished" v-bind:isWon="playerWon">
    </GameFinished>

    <div v-else>
        <GameObject
            v-bind:location="playerLocation"
            type="player"
            v-bind:image="playerImage">
        </GameObject>

        <Timer 
            v-bind:seconds="secondsRemaining">
        </Timer>

        <GameObject
            v-bind:location="finishLineLocation"
            type="finishline">
        </GameObject>
    </div>

</template>

<script>
import GameObject from './components/GameObject.vue';
import Timer from './components/Timer.vue';
import GameFinished from './components/GameFinished.vue';

const images = [ "daley-1.png", "daley-2.png", "daley-3.png" ];
let lastSwitch = (new Date()).getTime();

export default {
    name: 'App',

    components: {    
        GameObject,
        Timer,
        GameFinished
    },

    data: function() {                    
        return {
            playerX: 100,
            playerY: 100,
            speed: 0,
            toggleSpeed: 1,                    
            startTime: 0,
            timeNow: 0,
            finishLine: 500,
            isGameFinished: false,
            lastPressed: 0,
            playerWon: false,
            interval: 0,
            currentImageIdx: 0            
        };
    },

    created() {
        window.addEventListener('keydown', this.onKeyDown);
    },

    computed: {                    
        playerLocation: function() {
            return 'width:100px; height:100px; left:' + this.playerX + 'px; top:' + this.playerY + 'px; position: absolute;';
        },

        playerImage: function() {                        
            return require("./assets/" + images[this.currentImageIdx]);
        },

        finishLineLocation: function() {
            return 'width:3px; height:300px; left:' + this.finishLine + 'px; top:50px; border:1px solid #000; position: absolute;';
        },

        secondsRemaining: function() {
            const diff = (this.startTime - this.timeNow) / 1000;
            return Math.round(diff);
        }
    },

    methods: {
        init() {
            this.interval = setInterval(this.update, 10);
            this.startTime = new Date();
            this.startTime.setSeconds(this.startTime.getSeconds() + 20);
        },

        onKeyDown(e) {      
            if (this.lastPressed + 50 > Date.now()) {
                return;
            } else {
                this.lastPressed = Date.now();
            }

            switch (e.which) {
                case 37: // Left
                    this.playerRun(1); 
                    break;
                case 39: // Right
                    this.playerRun(2); 
                    break;
                default:
                    break;
            }
        },
        
        playerRun(toggle) {                        
            if (this.toggleSpeed !== toggle) {                           
                this.speed += 0.12;
                this.toggleSpeed = toggle;
            }
        },                  

        update() {
            this.playerX += this.speed;

            if (this.speed > 0) {
                this.speed -= 0.01;
            } else {
                this.speed = 0;
            }

            this.timeNow = new Date().getTime();
            
            if (this.playerX > this.finishLine) {
                this.isGameFinished = true;
                this.playerWon = this.isGameFinished && (this.secondsRemaining > 0);
                clearInterval(this.interval);
            }

            if (this.secondsRemaining <= 0) {
                this.isGameFinished = true;
                this.playerWon = this.isGameFinished && (this.secondsRemaining > 0);
                clearInterval(this.interval);
            }

            if (this.speed > 0) {
                const refreshSpeed = 1000 - (this.speed * 3);
                if ((new Date()).getTime() > lastSwitch + refreshSpeed) {
                    this.currentImageIdx = 
                        (this.currentImageIdx < images.length - 1) 
                            ? this.currentImageIdx + 1 : 0;
                    lastSwitch = new Date().getTime();
                }
            }
        }
    },

    mounted() {
        this.init();
    }
}
</script>

