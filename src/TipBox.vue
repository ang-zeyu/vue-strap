<template>
    <div class="alert container" :class="[boxStyle, addClass, {'alert-dismissible': dismissible}]" :style="customStyle">

        <h6 v-if="heading" class="headingDefault">{{ heading }}</h6>
        <div v-if="!heading" class="headingNotPresent"></div>

        <div class="contentContainer">
            <div v-if="!isDefault" class="icon-wrapper">
                <span v-html="iconType"></span>
            </div>
            <div class="contents">
                <slot></slot>
            </div>
            <button v-if="dismissible" type="button" class="close closeButton" data-dismiss="alert" aria-label="Close">
                <span aria-hidden="true">&times;</span>
            </button>
        </div>
    </div>
</template>

<script>
  import md from './utils/markdown.js'
  export default {
    props: {
      dismissible: {
        type: Boolean,
        default: false
      },
      backgroundColor: {
        type: String,
        default: null
      },
      borderColor: {
        type: String,
        default: null
      },
      borderLeftColor: {
        type: String,
        default: null
      },
      color: {
        type: String,
        default: null
      },
      icon: {
        type: String,
        default: null
      },
      type: {
        type: String,
        default: 'none'
      },
      addClass: {
        type: String,
        default: ''
      },
      heading: {
        type: String,
        default: null,
      }
    },
    computed: {
      isDefault() {
        return this.type === 'none'
      },
      boxStyle() {
        switch (this.type) {
          case 'warning':
            return 'alert-warning'
          case 'info':
          case 'definition':
            return 'alert-info'
          case 'success':
          case 'tip':
            return 'alert-success'
          case 'important':
          case 'wrong':
            return 'alert-danger'
          default:
            return 'alert-default'
        }
      },
      customStyle() {
        var style = {};
        if (this.backgroundColor) {
          style.backgroundColor = this.backgroundColor;
          style.borderColor = this.backgroundColor;
        }
        if (this.borderColor) {
          style.borderColor = this.borderColor;
        }
        if (this.borderLeftColor) {
          style.borderLeft = `5px solid ${this.borderLeftColor}`;
        }
        if (this.color) {
          style.color = this.color;
        }
        return style;
      },
      iconType() {
        if (this.icon) {
          return md.renderInline(this.icon);
        }
        switch (this.type) {
          case 'wrong':
            return '<i class="fas fa-times"></i>';
          case 'warning':
            return '<i class="fas fa-exclamation"></i>';
          case 'info':
            return '<i class="fas fa-info"></i>';
          case 'success':
            return '<i class="fas fa-check"></i>';
          case 'important':
            return '<i class="fas fa-flag"></i>';
          case 'tip':
            return '<i class="fas fa-lightbulb"></i>';
          case 'definition':
            return '<i class="fas fa-atlas"></i>';
          default:
            return '<i class="fas fa-exclamation"></i>';
        }
      }
    }
  }
</script>

<style scoped>
    .container {
        width: 100%;
        border-radius: 6px;
        padding: 0;
    }

    .headingDefault {
        display: inline-block;
        color: inherit;
        float: right;
        width: auto;
        padding: 2px 3px 3px 4px;
        border-width: 1px;
        border-style: solid;
        border-color: inherit;
        border-radius: 0 6px 0 6px;
        margin-top: -1px;
        margin-right: -1px;
        margin-bottom: 5px;
        background-color: rgba(255, 255, 255, 0.5);
    }

    .headingNotPresent {
        margin-bottom: 15px;
    }

    .contentContainer {
        width: 100%;
        display: flex;
        flex-direction: row;
        padding: 0 15px 15px 15px;
        margin: 0;
    }

    .alert-dismissible .closeButton {
        top: auto;
        padding-top: 0;
        padding-bottom: 0;
    }

    .icon-wrapper {
        display: flex;
        margin-right: .5em;
        width: 22px;
        height: 22px;
    }

    .contents {
        padding: 0 6px;
        width: 100%;
    }

    .alert-default {
        color: #24292e;
        background-color: #f6f8fa;
        border-color: #e8ebef;
    }
</style>
