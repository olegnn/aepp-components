<template>
  <component :is="to ? 'ae-link' : 'div'" class="ae-panel" :to="to">
    <div class="progressbar" :style="progressbarStyle(ratioTop)" />
    <div class="content">
      <header v-if="title || closeHandler">
        <h1>{{title}}</h1>
        <ae-button @click="closeHandler" plain size="small">
          <ae-icon slot='icon' name='close' />
        </ae-button>
      </header>
      <!-- This is the Body of the Panel -->
      <slot />
    </div>
    <div class="progressbar" :style="progressbarStyle(ratioBottom)" />
  </component>
</template>

<script>
  import AeButton from '../aeButton/aeButton.vue'
  import AeIcon from '../aeIcon/aeIcon.vue'
  import AeLink from '../aeLink/aeLink.vue'
  /**
   *
   */
  export default {
    name: 'ae-panel',
    props: {
      /**
       * Where does the Panel link to. its fed to a ae-link.
       */
      to: [String, Object],

      /**
       * The ratio of the progressbar on top between 0 and 1
       */
      ratioTop: { type: Number, required: false },

      /**
       * The ratio of the progressbar on bottom between 0 and 1
       */
      ratioBottom: { type: Number, required: false },

      /**
       * The title displayed in the panel
       */
      title: { type: String, required: false },

      /**
       * This handler is called when the user presses the close button
       */
      closeHandler: { type: Function, required: false }
    },
    components: { AeButton, AeIcon, AeLink },
    methods: {
      progressbarStyle (ratio) {
        const pc = ratio * 100
        return {
          display: typeof ratio === 'number' ? 'block' : 'none',
          backgroundImage:
            `linear-gradient(to right, var(--maegenta) ${pc}%, var(--aubergine) ${pc}%)`
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "../variables";
  @import "../mixins";

  .ae-panel {
    border-radius: 10px;
    box-shadow: 0 4px 8px 2px rgba(60, 60, 60, 0.1); // added 2px spread
    max-width: $container-width;
    margin: 20px auto;
    box-sizing: border-box;
    overflow: hidden;
    display: block;
    text-decoration: none;
    background-color: $white;
    color: inherit;
    @include belowDesktop {
      margin-left: 10px;
      margin-right: 10px;
    }
    @include phone {
      margin-top: 10px;
      margin-bottom: 10px;
    }

    .progressbar, .underline {
      content: "";
      display: block;
      width: 100%;
      height: 4px;
    }
    .underline {
      background-color: $maegenta;
    }

    .content {
      padding: 30px;
      @include phone {
        padding: 20px;
      }

      header {
        position: relative;

        h1 {
          font-size: 28px;
          line-height: 50px;
          font-weight: 500;
          margin: 0;
        }

        .ae-button {
          position: absolute;
          top: 0;
          right: 0;
        }
      }
    }
  }
</style>
