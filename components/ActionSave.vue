<template>
  <div>
    <div class="sr-only">
      <div
        v-if="standard"
        ref="image"
        class="w-screen h-screen"
        :class="gradient"
      />
      <div
        v-if="mesh"
        ref="image"
        class="w-screen h-screen"
        :style="gradient"
      />
      <div
        v-if="grainy"
        ref="image"
        class="relative w-screen h-screen"
        :class="gradient"
      >
        <div
          class="absolute inset-0 noise brightness-100 contrast-150 filter"
          :style="`--color: ${color}`"
        />
      </div>
    </div>

    <div class="flex items-center gap-2">
      <button
        v-if="standard"
        class="rounded-xl bg-gray-800/75 p-2.5 transition-colors hover:text-pink-500"
        aria-label="Copy Tailwind CSS class names"
        @click="handleTailwind"
      >
        <icon-tailwind class="w-4 h-4" />
      </button>

      <button
        v-if="standard || mesh"
        class="rounded-xl bg-gray-800/75 p-2.5 transition-colors hover:text-pink-500"
        aria-label="Copy CSS"
        @click="handleCode"
      >
        <icon-code class="w-4 h-4" />
      </button>

      <button
        class="rounded-xl bg-gray-800/75 p-2.5 transition-colors hover:text-pink-500"
        aria-label="Download image"
        @click="handleImage"
      >
        <icon-image class="w-4 h-4" />
      </button>
    </div>
  </div>
</template>

<script>
import { getBackgroundImage } from '@/utils/getColor'
import { createAndDownloadImage } from '@/utils/downloadImage'

export default {
  props: {
    name: {
      type: String,
      required: true
    },
    gradient: {
      type: String,
      required: true
    },
    color: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'standard'
    }
  },
  computed: {
    standard () {
      return this.type === 'standard'
    },
    mesh () {
      return this.type === 'mesh'
    },
    grainy () {
      return this.type === 'grainy'
    }
  },
  methods: {
    handleTailwind () {
      this.code = this.gradient
      navigator.clipboard.writeText(this.code)

      this.showToast()
    },
    handleCode () {
      this.code = this.mesh
        ? this.gradient
        : getBackgroundImage(this.$refs.image)

      navigator.clipboard.writeText(this.code)

      this.showToast()
    },
    handleImage () {
      createAndDownloadImage(this.$refs.image, this.name)
    },
    showToast () {
      this.$toast.success('Copied to Clipboard')
    }
  }
}
</script>

<style lang="postcss" scoped>
/* https://css-tricks.com/grainy-gradients/ */

.noise {
  background: linear-gradient(180deg, var(--color), transparent),
    url(https://grainy-gradients.vercel.app/noise.svg);
}
</style>
