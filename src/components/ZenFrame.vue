<template>
    <div class = "zen-frame" >
        <div class = "zen-content" >
            <h2>{{ data.title }}</h2>
            <p>{{ data.content }}</p>
        </div>

        <div class = "zen-button--container" >
            <button class = "zen-button zen-button--next" 
                    @click = "nextFrame" 
                    v-if = "index === length-1" >
                Next
            </button>
            <span v-else-if= "index === 0" >
                <button class = "zen-button--half zen-button--previous" 
                        @click = "previousFrame" >
                    Previous
                </button>
                <button class = "zen-button--half zen-button--close" 
                        @click = "closeModal" >
                    Close
                </button>
            </span>
            <span v-else>
                <button class = "zen-button--half zen-button--previous" 
                        @click = "previousFrame" >
                    Previous
                </button>
                <button class = "zen-button--half zen-button--next" 
                        @click = "nextFrame" >
                    Next
                </button>
            </span>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['index', 'key', 'data', 'length', 'visible'],
        // Active frame handling will be cleaner once I implement vuex!
        methods: {
            // Recursively checks for @classString in node.classList
            // If not found, checks node.offsetParent
            findParent(node, classString) {
                if ( node.classList && node.classList.contains(classString) ) {
                    return node
                } else if ( node.offsetParent ) {
                    return this.findParent(node.offsetParent, classString)
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
                this.$emit('visible', false)
            }
        }
    }
</script>

<style lang="scss">
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

                &:hover {
                    cursor: pointer;
                }

                &--half {
                    width: 50%;
                    padding: 10px;
                    float: left;
                    color: white;

                    &:hover {
                        cursor: pointer;
                    }
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
</style>
