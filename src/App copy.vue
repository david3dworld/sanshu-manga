<template>
  <div
    id="app"
    :class="{ 'has-mouse': hasMouse }"
    @touchstart="hasMouse = false"
  >
    
    <Flipbook
      class="flipbook"
      :pages="pages"
      :pagesHiRes="pagesHiRes"
      :startPage="pageNum"
      v-slot="flipbook"
      ref="flipbook"
      @flip-left-start="onFlipLeftStart"
      @flip-left-end="onFlipLeftEnd"
      @flip-right-start="onFlipRightStart"
      @flip-right-end="onFlipRightEnd"
      @zoom-start="onZoomStart"
      @zoom-end="onZoomEnd"
    >
      <div class="action-bar">
        <left-icon
          class="btn left"
          :class="{ disabled: !flipbook.canFlipLeft }"
          @click="flipbook.flipLeft"
        />
        <plus-icon
          class="btn plus"
          :class="{ disabled: !flipbook.canZoomIn }"
          @click="flipbook.zoomIn"
        />
        <span class="page-num">
          Page {{ flipbook.page }} of {{ flipbook.numPages }}
        </span>
        <minus-icon
          class="btn minus"
          :class="{ disabled: !flipbook.canZoomOut }"
          @click="flipbook.zoomOut"
        />
        <right-icon
          class="btn right"
          :class="{ disabled: !flipbook.canFlipRight }"
          @click="flipbook.flipRight"
        />
      </div>
    </Flipbook>    
  </div>
</template>

<script lang="coffee">
import 'vue-material-design-icons/styles.css'
import Ribbon from 'vue-ribbon'
import LeftIcon from 'vue-material-design-icons/ChevronLeftCircle'
import RightIcon from 'vue-material-design-icons/ChevronRightCircle'
import PlusIcon from 'vue-material-design-icons/PlusCircle'
import MinusIcon from 'vue-material-design-icons/MinusCircle'
import Flipbook from './Flipbook'

export default
  name: 'app'
  components: { Flipbook, LeftIcon, RightIcon, PlusIcon, MinusIcon, Ribbon }
  data: ->
    pages: [],
    pagesHiRes: [],
    hasMouse: true
    pageNum: null
  methods:
    onFlipLeftStart: (page) -> console.log 'flip-left-start', page
    onFlipLeftEnd: (page) ->
      console.log 'flip-left-end', page
      window.location.hash = '#' + page
    onFlipRightStart: (page) -> console.log 'flip-right-start', page
    onFlipRightEnd: (page) ->
      console.log 'flip-right-end', page
      window.location.hash = '#' + page
    onZoomStart: (zoom) -> console.log 'zoom-start', zoom
    onZoomEnd: (zoom) -> console.log 'zoom-end', zoom
    setPageFromHash: ->
      n = parseInt window.location.hash.slice(1), 10
      @pageNum = n if isFinite n
  mounted: ->
    window.addEventListener 'keydown', (ev) =>
      flipbook = @$refs.flipbook
      return unless flipbook
      flipbook.flipLeft() if ev.keyCode == 37 and flipbook.canFlipLeft
      flipbook.flipRight() if ev.keyCode == 39 and flipbook.canFlipRight

    # Simulate asynchronous pages initialization
    setTimeout (=>
      @pages = [
        null
        'images/1.jpg'
        'images/100.jpg'
        'images/2.jpg'
        'images/3.jpg'
        'images/4.jpg'
        'images/5.jpg'
        'images/6.jpg'
        'images/7.jpg'
        'images/8.jpg'
        'images/9.jpg'
        'images/10.jpg'
        'images/11.jpg'
        'images/12.jpg'
        'images/13.jpg'
        'images/14.jpg'
        'images/15.jpg'
        'images/16.jpg'
        'images/17.jpg'
        'images/18.jpg'
        'images/19.jpg'
        'images/20.jpg'
        'images/21.jpg'
        'images/22.jpg'
        'images/23.jpg'
        'images/24.jpg'
        'images/25.jpg'
        'images/26.jpg'
        'images/27.jpg'
        'images/28.jpg'
        'images/29.jpg'
        'images/30.jpg'
        'images/31.jpg'
        'images/32.jpg'
        'images/33.jpg'
        'images/34.jpg'
        'images/35.jpg'
        'images/36.jpg'
        'images/37.jpg'
      ]
      @pagesHiRes = [
        null
        'images-large/1.jpg'
        'images-large/100.jpg'
        'images-large/2.jpg'
        'images-large/3.jpg'
        'images-large/4.jpg'
        'images-large/5.jpg'
        'images-large/6.jpg'
        'images-large/7.jpg'
        'images-large/8.jpg'
        'images-large/9.jpg'
        'images-large/10.jpg'
        'images-large/11.jpg'
        'images-large/12.jpg'
        'images-large/13.jpg'
        'images-large/14.jpg'
        'images-large/15.jpg'
        'images-large/16.jpg'
        'images-large/17.jpg'
        'images-large/18.jpg'
        'images-large/19.jpg'
        'images-large/20.jpg'
        'images-large/21.jpg'
        'images-large/22.jpg'
        'images-large/23.jpg'
        'images-large/24.jpg'
        'images-large/25.jpg'
        'images-large/26.jpg'
        'images-large/27.jpg'
        'images-large/28.jpg'
        'images-large/29.jpg'
        'images-large/30.jpg'
        'images-large/31.jpg'
        'images-large/32.jpg'
        'images-large/33.jpg'
        'images-large/34.jpg'
        'images-large/35.jpg'
        'images-large/36.jpg'
        'images-large/37.jpg'
        
      ]
    ), 1

    window.addEventListener 'hashchange', @setPageFromHash
    @setPageFromHash()
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #333;
  color: #ccc;
  overflow: hidden;
}

a {
  color: inherit;
}

.action-bar {
  width: 100%;
  height: 30px;
  padding: 10px 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.action-bar .btn {
  font-size: 30px;
  color: #999;
}

.action-bar .btn svg {
  bottom: 0;
}

.action-bar .btn:not(:first-child) {
  margin-left: 10px;
}

.has-mouse .action-bar .btn:hover {
  color: #ccc;
  filter: drop-shadow(1px 1px 5px #000);
  cursor: pointer;
}

.action-bar .btn:active {
  filter: none !important;
}

.action-bar .btn.disabled {
  color: #666;
  pointer-events: none;
}

.action-bar .page-num {
  font-size: 12px;
  margin-left: 10px;
}

.flipbook .viewport {
  width: 90vw;
  height: calc(100vh - 50px - 40px);
}

.flipbook .bounding-box {
  box-shadow: 0 0 20px #000;
}

.credit {
  font-size: 12px;
  line-height: 20px;
  margin: 10px;
}
</style>
