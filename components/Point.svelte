<script>
  import { Layer } from "svelte-canvas"
  export let x = 0
  export let y = 0
  export let r = 0
  export let fill
  // export let width

  $: render = ({ context }) => {
    context.beginPath()
    context.arc(x, y, r, 0, 2 * Math.PI)
    context.fillStyle = fill
    context.fill()
    // context.stroke()
    // context.strokeStyle = "#e0e1e2"
  }
</script>

<Layer {render} />
