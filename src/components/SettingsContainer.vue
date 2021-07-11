<template>
    <div class="settingsContainer">
        <div class="checkbox">
            <samp class="checkbox__text">Celsius</samp>
            <label class="checkbox__switch">
                <input
                        type="checkbox"
                        v-model="tempSettings"
                        @change="temp"
                        true-value="imperial"
                        false-value="metric"
                >
                <span class="checkbox__slider slider "></span>
            </label>
            <samp class="checkbox__text">Fahrenheit</samp>
        </div>
        |
        <div class="checkbox">
            <samp class="checkbox__text">Day</samp>
            <label class="checkbox__switch">
                <input
                        type="checkbox"
                        v-model="dayNight"
                        @change="timesOfDay"
                        true-value="day"
                        false-value="night "
                >
                <span class="checkbox__slider slider "></span>
            </label>
            <samp class="checkbox__text">Night</samp>
        </div>
    </div>
</template>

<script lang="ts">

export default {
    data() {
        return {
            tempSettings: 'metric',
            dayNight: 'day'
        }
    },
    methods: {
        temp(val: any): void {
            this.$emit('setTemp', this.tempSettings)
        },
        timesOfDay(val: any): void {

            this.$emit("timesOfDay", this.dayNight)
        },
    }
}
</script>

<style scoped lang="sass">
    @import "../assets/colors"

    .settingsContainer
        display: flex
        align-items: center
        justify-content: center

        .checkbox
            display: flex
            align-items: center
            justify-content: center

            &__text
                margin: 0 5px

            &__switch
                position: relative
                display: inline-block
                width: 52px
                height: 24px

                & input
                    opacity: 0
                    width: 0
                    height: 0

            &__slider
                position: absolute
                cursor: pointer
                top: 0
                left: 0
                right: 0
                bottom: 0
                background-color: $element-color
                -webkit-transition: .4s
                transition: .4s
                border-radius: 34px

                &:before
                    position: absolute
                    content: ""
                    height: 18px
                    width: 18px
                    left: 4px
                    bottom: 4px
                    background-color: white
                    -webkit-transition: .4s
                    transition: .4s
                    border-radius: 50%

    input
        &:checked + .slider
            background-color: $element-color

        &:focus + .slider
            box-shadow: 0 0 1px $element-color

        &:checked + .slider:before
            -webkit-transform: translateX(26px)
            -ms-transform: translateX(26px)
            transform: translateX(26px)
</style>
