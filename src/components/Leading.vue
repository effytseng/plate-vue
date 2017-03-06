<template>
  <div class="leading">
    <div class="leading-container">
      <div class="leading-slideshow" v-if="type === 'slideshow' && slideshowImgs.length > 0">
        <app-slider :option="sliderOption">
          <template scope="props">
            <swiper-slide :is="props.slide" v-for="(o, i) in slideshowImgs">
              <img :data-src="getValue(o, [ 'image', 'url' ])"
                    :data-srcset="`${getValue(o, [ 'image', 'resizedTargets', 'mobile', 'url' ])} 800w,
                                  ${getValue(o, [ 'image', 'resizedTargets', 'tablet', 'url' ])} 1200w,
                                  ${getValue(o, [ 'image', 'resizedTargets', 'desktop', 'url' ])} 2000w`"
                    class="swiper-lazy" />
              <div class="swiper-lazy-preloader"></div>
            </swiper-slide>
          </template>
        </app-slider>
      </div>
      <div class="leading-image" v-else-if="type === 'image' && leadingImg">
        <div class="img">
          <img :src="getValue(leadingImg, [ 'image', 'url' ])"
                :srcset="`${getValue(leadingImg, [ 'image', 'resizedTargets', 'mobile', 'url' ])} 800w,
                          ${getValue(leadingImg, [ 'image', 'resizedTargets', 'tablet', 'url' ])} 1200w,
                          ${getValue(leadingImg, [ 'image', 'resizedTargets', 'desktop', 'url' ])} 2000w`" />
        </div>
      </div>
      <div class="leading-video" v-else-if="type === 'video' && leadingVideo">
        <div class="video">
          <video controls ref="video">
            <source :src="getValue(leadingVideo, [ 'video', 'url' ])" :type="getValue(leadingVideo, [ 'video', 'filetype' ])" />
          </video>
        </div>
      </div>
      <div class="leading-embedded" v-else-if="type === 'embedded' && leadingEmedded">
        <div className = "embedded" v-html="leadingEmedded"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { getValue } from '../utils/comm'
import _ from 'lodash'
import Slider from './Slider.vue'

export default {
  components: {
    'app-slider': Slider
  },
  computed: {
    sliderOption() {
      return {
        paginationable: false,
        paginationClickable: true,
        paginationHide: false,
        setNavBtn: true
      }
    },
    slideshowImgs() {
      return _.get(this.mediaData, [ 'images', 'items' ], [])
    },
    leadingImg() {
      return _.get(this.mediaData, [ 'heroImage' ])
    },
    leadingVideo() {
      return _.get(this.mediaData, [ 'heroVideo' ])
    },
    leadingEmedded() {
      return _.get(this.mediaData, [ 'embedded' ])
    }
  },
  methods: {
    getValue
  },
  name: 'leading',
  props: {
    mediaData: {
      default: () => { return {} }
    },
    type: {
      default: () => { return {} }
    }
  },
}
  
</script>
<style lang="stylus" scoped>
  .leading
    width 50%
    height 0
    margin 0 auto
    .leading-container
      width 100%
      position relative
      padding-bottom 56.25%
      padding-top 25px
      height 100%
      .leading-slideshow
        height 100%
        position absolute
        top 0
        left 0
        width 100%
        height 100%        
        .swiper-container
          height 100%
          .swiper-wrapper
            height 100%
            .swiper-slide
              img
                width 100%
                object-fit contain
                max-height 100%
      .img
        height 100%
        position absolute
        top 0
        left 0
        width 100%
        height 100% 
        img
          width 100%
          object-fit contain
          max-height 100%
      .video
        height 100%
        position absolute
        top 0
        left 0
        width 100%
        height 100%       
        video
          width 100%
          object-fit contain
          max-height 100%



</style>