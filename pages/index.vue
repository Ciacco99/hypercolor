<template>
  <div>
    <content-banner title="Hypercolor" subtitle="Gradients for Tailwind CSS">
      A curated collection of beautiful Tailwind CSS gradients using the full
      range of Tailwind CSS colors. Easily copy and paste the class names, CSS
      or even save the gradients as an image.
    </content-banner>

    <gradient-grid :gradients="gradients" />
  </div>
</template>

<script>
import { gradientsCore } from '@/assets/data/gradientsCore'

export default {
  name: 'HomePage',
  computed: {
    gradients () {
      return gradientsCore
    }
  }
}
</script>
