<template src="./index.html"></template>

<script>
import Vue from 'vue'
import SvgIcon from 'vue-svgicon'
import '../SvgIcons/index.js'

Vue.use(SvgIcon, {
    tagName: 'svgicon'
})

export default {
    name: 'scheme',
    data () {
        return {
            isHover: false
        }
    },
    methods: {
        addHoverEventListener () {
            let triggerBoxes = document.getElementsByClassName('popup-trigger-box')

            Array.from(triggerBoxes).forEach((item, index) => {
                let popupItem = document.getElementById(item.dataset.popupId)

                item.addEventListener('mouseover', (event) => {
                    this.isHover = true
                    popupItem.style.display = 'block'
                    let bodyRect = document.body.getBoundingClientRect()
                    let popupItemRect = popupItem.getBoundingClientRect()
                    let elemRect = item.getBoundingClientRect()

                    if (bodyRect.height - elemRect.y < popupItemRect.height) {
                        popupItem.style.top = elemRect.y - bodyRect.y - 90 + 'px'
                    } else {
                        popupItem.style.top = elemRect.y - bodyRect.y + 'px'
                    }
                    popupItem.style.left = elemRect.x - bodyRect.x + 'px'
                })
                item.addEventListener('mouseleave', (event) => {
                    this.isHover = false
                    popupItem.style.display = 'none'
                })
            })
        }
    },
    mounted () {
        this.addHoverEventListener()
    },
    updated () {}
}
</script>

<style lang="scss" src="./index.scss"></style>
