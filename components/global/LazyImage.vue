<template>
  <img :data-src="lozadLazySrc" :style="style" />
</template>

<script>
// Component from https://markus.oberlehner.net/blog/lazy-loading-responsive-images-with-vue/

import lozad from 'lozad'

export default {
  name: 'LazyImage',
  props: {
    backgroundColor: {
      type: String,
      default: '#efefef',
    },
    height: {
      type: Number,
      default: null,
    },
    lozadLazySrc: {
      type: String,
      default: null,
    },
    lozadLazySrcset: {
      type: String,
      default: null,
    },
    width: {
      type: Number,
      default: null,
    },
    index: {
      type: Number,
    },
    openGallery: {
      type: Function,
    },
    loading: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    aspectRatio() {
      // Calculate the aspect ratio of the image
      // if the width and the height are given.
      if (!this.width || !this.height) return null

      return (this.height / this.width) * 100
    },
    style() {
      // The background color is used as a
      // placeholder while loading the image.
      // You can use the dominant color of the
      // image to improve perceived performance.
      // See: https://manu.ninja/dominant-colors-for-lazy-loading-images/
      const style = { backgroundColor: this.backgroundColor }

      if (this.width) style.width = `${this.width}px`

      // If the image is still loading and an
      // aspect ratio could be calculated, we
      // apply the calculated aspect ratio by
      // using padding top.
      const applyAspectRatio = this.loading && this.aspectRatio
      if (applyAspectRatio) {
        // Prevent flash of unstyled image
        // after the image is loaded.
        style.height = 0
        // Scale the image container according
        // to the aspect ratio.
        style.paddingTop = `${this.aspectRatio}%`
      }

      return style
    },
  },
  methods: {
    setLoadingState() {
      let isLoading = this.loading
      isLoading = false
      this.$emit('image-loaded', isLoading)
    },
  },
  mounted() {
    // As soon as the <img> element triggers
    // the `load` event, the loading state is
    // set to `false`, which removes the apsect
    // ratio we've applied earlier.
    // const setLoadingState = () => {
    //   this.$emit('load', false)
    // }
    this.$el.addEventListener('load', this.setLoadingState)

    // We remove the event listener as soon as
    // the component is destroyed to prevent
    // potential memory leaks.
    this.$once('hook:destroyed', () => {
      this.$el.removeEventListener('load', this.setLoadingState)
    })

    // We initialize Lozad.js on the root
    // element of our component.
    const observer = lozad(this.$el, {
      rootMargin: '10px 0px',
      threshold: 0.001,
      // enableAutoReload: true,
    })
    observer.observe()
  },
}
</script>

<style>
/* Responsive image styles. */
.AppImage {
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  vertical-align: middle;
}
</style>