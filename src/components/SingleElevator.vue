<template>
    <div class="elevator-container">
        <div v-for="levelNumber in numberOfLevels" :key="levelNumber">
            <SingleCabineCell
                :flexOrder="levelNumber"
                :movingDirection="movingDirection"
                :elevatorPosition="elevatorPosition"
                :finishedCallAnimation="finishedCallAnimation"
            />
        </div>
    </div>
</template>

<script>
import SingleCabineCell from "./SingleCabineCell";
export default {
    name: "SingleElevator",
    data() {
        return {
            elevatorPosition: 1,
            movingDirection: {
                level: null,
                direction: "🟢",
            },
            finishedCallAnimation: false,
            isBusy: false,
        };
    },
    components: {
        SingleCabineCell,
    },
    props: ['id', 'numberOfLevels'],
    watch: {
        elevatorPosition() {
            localStorage.setItem("elevator-" + this.id, this.elevatorPosition)
        },
        
        isBusy() {
            localStorage.setItem("state", this.isBusy)
        }
    },
    methods: {
        displayElevatorState(isReady) {
            this.movingDirection.level = null;

            if (isReady) {
                this.finishedCallAnimation = false;
                this.movingDirection.direction = "🟢";
            } else {
                this.finishedCallAnimation = true;
                this.movingDirection.direction = "🔴";
            }
        },

        endCall(currentCall) {
            this.displayElevatorState(true);
            this.isBusy = false;
    
            this.$parent.nextCall(currentCall);
        },

        indicateFinish(interval, destinationLevel) {
            clearInterval(interval);

            this.displayElevatorState(false);

            setTimeout(() => this.endCall(destinationLevel), 3000);
        },

        initNewCall(destinationLevel) {
            this.isBusy = true;

            this.movingDirection.level = destinationLevel;
            this.movingDirection.direction = (this.elevatorPosition < destinationLevel) ? "⬆" : (this.elevatorPosition > destinationLevel) ? "⬇" : ""

            let interval = setInterval(() => {
                if (this.elevatorPosition === destinationLevel) {
                    this.indicateFinish(interval, destinationLevel);
                } else if (this.elevatorPosition < destinationLevel) {
                    this.elevatorPosition += 1;
                } else if (this.elevatorPosition > destinationLevel) {
                    this.elevatorPosition -= 1;
                } else {
                    return;
                }
            }, 1000);
        },
        
        elevatorIsBusy() {
            return this.isBusy;
        },

        getPosition() {
            return this.elevatorPosition
        },

        
    },

    mounted() {
        if (localStorage["elevator-" + this.id]) {
            let position = JSON.parse(localStorage.getItem("elevator-" + this.id))
            this.elevatorPosition = position
        }
        if (localStorage.state) {
            this.isBusy = JSON.parse(localStorage.getItem("state"))
        }
    },
};
</script>

<style scoped>
.elevator-container {
    display: flex;
    flex-direction: column-reverse;
    flex-wrap: wrap;
}
</style>
