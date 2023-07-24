<template></template>
<script>
export default {
    props: ['weather', 'tempFromFav'],
    emits: ['background'],
    data() {
        return {
            temp: null,
        }
    },
    methods: {
        computeBackground(picture) {
            if (this.temp < 32) {
                this.picture = 'https://images.unsplash.com/photo-1613156976910-6ca4355d2c10?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80'
            } else if (this.temp >= 32 && this.temp < 50) {
                this.picture = 'https://images.unsplash.com/photo-1581196607303-95c00f31c676?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80'
            } else if (this.temp >= 50 && this.temp < 68) {
                this.picture = 'https://images.unsplash.com/photo-1510739859545-e7b9e979de86?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=774&q=80'
            } else if (this.temp >= 68 && this.temp < 86) {
                this.picture = 'https://images.unsplash.com/photo-1600647993560-11a92e039466?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=387&q=80'
            } else {
                this.picture = 'https://images.unsplash.com/photo-1469122312224-c5846569feb1?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1289&q=80'
            }
            this.$emit('background', this.picture)
        }
    },
    watch: {
        weather(val) {
            if (val) {
                this.temp = val.temp;
                this.computeBackground(this.sendBG);
            }
        },
        tempFromFav(val) {
            if (val) {
                this.temp = val;
                this.computeBackground(val)
            }
        }
    }
}
</script>
// 0c = 32f
// 10c = 50f
// 20c = 68f
// 30c = 86