<template>
  <div>
    <div style="position: absolute;" v-drag>
    </div>
  </div>
</template>

<script>
import drag from '@branu-jp/v-drag'

export default {
  directives: {
    drag
  }
}
</script>