<template>
    <div class="zen-window" 
         :class="{ active: visible }">
        <div class="zen-wrapper">
            <div class="zen-frame" 
                v-for="(frame, index) in reverseContent">
                <div class=" zen-content">
                    <h2>{{ frame.title }}</h2>
                    <p>{{ frame.content }}</p>
                </div>

                <div class="zen-button--container">
                    <button class="zen-button zen-button--next" 
                            @click="nextFrame" 
                            v-if="index === content.length - 1" >
                        Next
                    </button>
                    <span v-else-if="index === 0">
                        <button class="zen-button--half zen-button--previous" 
                                @click="previousFrame">
                            Previous
                        </button>
                        <button class="zen-button--half zen-button--close" 
                                @click="closeModal">
                            Close
                        </button>
                    </span>
                    <span v-else>
                        <button class="zen-button--half zen-button--previous" 
                                @click="previousFrame">
                            Previous
                        </button>
                        <button class="zen-button--half zen-button--next" 
                                @click="nextFrame">
                            Next
                        </button>
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['visible', 'content'],
        computed: {
            reverseContent: function() {
                return this.content.reverse()
            }
        },
        methods: {
            // Recursively checks for @classString in node.classList
            // If not found, checks node.offsetParent
            findParent(node, classString) {
                if ( node.classList ) {
                    if (node.classList.contains(classString)) {
                        return node
                    } else {
                        return this.findParent(node.offsetParent, classString)
                    }
                } else {
                    return false
                }
            },
            // Hides current frame by adding class close-frame
            nextFrame(e) {
                let parent = this.findParent(e.target, 'zen-frame')
                
                if ( parent ) {
                    parent.classList.add('close-frame')
                } else {
                    console.log('Error: No such parent. (nextFrame)')
                }  
            },
            // Shows previous frame by removing class 'close-frame'
            previousFrame(e) {
                let previousFrame = this.findParent(e.target, 'zen-frame')

                if ( parent ) {
                    previousFrame.nextElementSibling.classList.remove('close-frame')
                } else {
                    console.log('Error: No such parent. (previousFrame)')
                }
            },
            // Shows all frames by removing class 'close-frame'
            resetTabs() {
                let closedFrames = document.querySelectorAll('.zen-frame.close-frame')
                closedFrames.forEach( (frame) => {
                    frame.classList.remove('close-frame')
                })
            },
            closeModal() {
                this.resetTabs()
                this.$emit('modalVisible', false)
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

        .zen-frame {
            width: 100%;
            height: 100%;
            background-color: white;
            border: 1px solid black;
            border-radius: 10px;
            overflow: hidden;
            padding: 20px;
            position: absolute;

            &.close-frame {
                display: none;
            }

            .zen-content {
                display: block;
            }

            .zen-button--container {
                position: absolute;
                bottom: 0;
                left: 0;
                width: 100%;

                .zen-button {
                    width: 100%;
                    padding: 10px;
                    color: white;

                    &--half {
                        width: 50%;
                        padding: 10px;
                        float: left;
                        color: white;
                    }

                    &--next {
                        background-color: green;
                    }

                    &--previous {
                        background-color: blue;
                    }

                    &--close {
                        background-color: red;
                    }
                }
            }
        }
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
