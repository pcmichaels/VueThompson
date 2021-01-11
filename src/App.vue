<template>
    <GameObject
        v-bind:location="playerLocation"
        type="player">
    </GameObject>

    <Timer 
        v-bind:seconds="secondsRemaining">
    </Timer>

    <GameObject
        v-bind:location="finishLineLocation"
        type="finishline">
    </GameObject>

    <GameFinished 
        v-if="isGameFinished" v-bind:isWon="playerWon">
    </GameFinished>
</template>

<script>
    import GameObject from './components/GameObject.vue';
    import Timer from './components/Timer.vue';
    import GameFinished from './components/GameFinished.vue';

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
            isGameFinished: false
        };
    },

    created() {
        window.addEventListener('keydown', this.onKeyDown);
    },

    computed: {                    
        playerLocation: function() {
            return 'width:10px; height:10px; left:' + this.playerX + 'px; top:' + this.playerY + 'px; border:1px solid #000; position: absolute;';
        },

        finishLineLocation: function() {
            return 'width:3px; height:300px; left:' + this.finishLine + 'px; top:50px; border:1px solid #000; position: absolute;';
        },

        secondsRemaining: function() {
            const diff = (this.startTime - this.timeNow) / 1000;
            return Math.round(diff);
        },

        playerWon: function() {
            return this.isGameFinished && (this.secondsRemaining > 0);
        }
    },

    methods: {
        init() {
            setInterval(this.update, 10);
            this.startTime = new Date();
            this.startTime.setSeconds(this.startTime.getSeconds() + 20);
        },

        onKeyDown(e) {                        
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

            // Collision check
            if (this.playerX > this.finishLine) {
                this.isGameFinished = true;
            }
        }
    },

    mounted() {
        this.init();
    }
}
</script>

