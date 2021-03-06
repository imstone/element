<template>
  <div class="el-switch" :class="{ 'is-disabled': disabled, 'el-switch--wide': hasText }">
    <div class="el-switch__mask" v-show="disabled"></div>
    <input class="el-switch__input" type="checkbox" v-model="value" :name="name" :disabled="disabled" style="display: none;">
    <span class="el-switch__core" ref="core" @click="handleMiscClick" :style="{ 'width': coreWidth + 'px' }">
      <span class="el-switch__button" ref="button"></span>
    </span>
    <transition name="label-fade">
      <div
        class="el-switch__label el-switch__label--left"
        v-show="value"
        @click="handleMiscClick"
        :style="{ 'width': coreWidth + 'px' }">
        <i :class="[onIconClass]" v-if="onIconClass"></i>
        <span v-if="!onIconClass && onText">{{ onText }}</span>
      </div>
    </transition>
    <transition name="label-fade">
      <div
        class="el-switch__label el-switch__label--right"
        v-show="!value"
        @click="handleMiscClick"
        :style="{ 'width': coreWidth + 'px' }">
        <i :class="[offIconClass]" v-if="offIconClass"></i>
        <span v-if="!offIconClass && offText">{{ offText }}</span>
      </div>
    </transition>
  </div>
</template>

<script>
  export default {
    name: 'el-switch',
    props: {
      value: {
        type: Boolean,
        default: true
      },
      disabled: {
        type: Boolean,
        default: false
      },
      width: {
        type: Number,
        default: 0
      },
      onIconClass: {
        type: String,
        default: ''
      },
      offIconClass: {
        type: String,
        default: ''
      },
      onText: {
        type: String,
        default: 'ON'
      },
      offText: {
        type: String,
        default: 'OFF'
      },
      onColor: {
        type: String,
        default: ''
      },
      offColor: {
        type: String,
        default: ''
      },
      name: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        coreWidth: 0
      };
    },
    computed: {
      hasText() {
        return this.onText || this.offText;
      }
    },
    watch: {
      value(val) {
        if (this.onColor || this.offColor) {
          this.handleCoreColor();
        }
        this.handleButtonTransform();
        this.$emit('change', val);
      }
    },
    methods: {
      handleMiscClick() {
        if (!this.disabled) {
          this.$emit('input', !this.value);
        }
      },
      handleButtonTransform() {
        this.$refs.button.style.transform = this.value ? `translate3d(${ this.coreWidth - 20 }px, 2px, 0)` : 'translate3d(2px, 2px, 0)';
      },
      handleCoreColor() {
        this.$refs.core.style.borderColor = this.value ? this.onColor : this.offColor;
        this.$refs.core.style.backgroundColor = this.value ? this.onColor : this.offColor;
      }
    },
    mounted() {
      if (this.width === 0) {
        this.coreWidth = this.hasText ? 58 : 46;
      }
      this.handleButtonTransform();
      if ((this.onColor || this.offColor) && !this.disabled) {
        this.handleCoreColor();
      }
    }
  };
</script>
