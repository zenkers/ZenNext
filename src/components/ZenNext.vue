<template>
    <div class="zen-window" 
         :class="{ active: visible }">
        <div class="zen-wrapper">
            <zen-frame v-for="(frame, index) in reverseContent"
                       :data = "frame"
                       :index = "index"
                       :length = "modalContent.length"
                       @visible="visible = $event" >
            </zen-frame>
        </div>
    </div>
</template>

<script>
    import ZenFrame from './ZenFrame.vue'
    export default {
        props: ['visible', 'modalContent'],
        components: {
            'zen-frame' : ZenFrame
        },
        computed: {
            reverseContent: function() {
                return this.modalContent.reverse()
            }
        }
    }
</script>

<style lang="scss">

    .zen-window {
        position: fixed;
        margin: 0;
        top: 50%;
        left: 50%;
        -webkit-transform: translate(-50%,-50%);
        transform: translate(-50%,-50%);
        width: 450px;
        margin: 0 auto;
        display: none;

        &.active {
            display: block;
        }

        .zen-wrapper {
            animation-duration: .5s;
            animation-fill-mode: both;
            animation-name: slideInDown;
            min-height: 300px;
        }
    }

    @keyframes slideInDown {
        from {
            transform: translate3d(0, -100%, 0);
            visibility: visible;
        }
        to {
            transform: translate3d(0, 0, 0);
        }
    }

</style>
