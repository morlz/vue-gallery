<template>
  <div id="blueimp-gallery" class="blueimp-gallery blueimp-gallery-controls">
    <div class="slides"></div>
    <h3 class="title"></h3>
    <a class="prev">‹</a>
    <a class="next">›</a>
    <a class="close">×</a>
    <ol class="indicator"></ol>
  </div>
</template>

<script>
  import 'blueimp-gallery/css/blueimp-gallery.min.css';
  import 'blueimp-gallery/js/blueimp-gallery-fullscreen.js';
  import blueimp from 'blueimp-gallery/js/blueimp-gallery.js';

  export default {
    props: {
      images: {
        type: Array,
        default() {
          return [];
        },
      },

      options: {
        type: Object,
        default() {
          return {};
        },
      },

      index: {
        type: Number,
      },
    },

    data() {
      return {
        instance: null,
      };
    },

    watch: {
      index(value) {
        if (value !== null) {
          this.open(value);
        } else {
          if (this.instance) {
            this.instance.close();
          }

          this.$emit('close');
        }
      },
    },

    destoryed() {
      this.instance.close();
      this.instance = null;
    },

    methods: {
      open(index) {
        const instance = typeof blueimp.Gallery !== 'undefined' ? blueimp.Gallery : blueimp;

        this.instance = instance(this.images, Object.assign({
          toggleControlsOnReturn: false,
          toggleControlsOnSlideClick: false,
          closeOnSlideClick: false,
          index,
          onopen: () => this.$emit('onopen'),
          onopened: () => this.$emit('onopened'),
          onslide: (index, slide) => this.$emit('onslide', { index, slide }),
          onslideend: (index, slide) => this.$emit('onslideend', { index, slide }),
          onslidecomplete: (index, slide) => this.$emit('onslidecomplete', { index, slide }),
          onclose: () => this.$emit('close'),
          onclosed: () => this.$emit('onclosed'),
        }, this.options));
      },
    },
  };
</script>
