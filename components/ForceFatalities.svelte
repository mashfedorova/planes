<script>
  import { Canvas } from "svelte-canvas"
  import { Layer } from "svelte-canvas"
  import {
    forceSimulation,
    forceCollide,
    forceX,
    forceY,
    scaleLinear,
    extent,
    scaleSqrt,
  } from "d3"

  import Point from "./Point.svelte"

  export let data
  let height = 1200
  let width = 700

  $: yScale = scaleLinear()
    .domain(extent(data, (d) => d.year))
    .range([100, height - 100])

  $: radiusScale = scaleSqrt()
    .domain(extent(data, (d) => d.injuries))
    .range([3, 25])

  $: simulation = forceSimulation(data)
    .force(
      "y",
      forceY()
        .y((d) => yScale(d.year))
        .strength(1.5)
    )
    .force("x", forceX(350).strength(1))
    .force(
      "collide",
      forceCollide().radius((d) => radiusScale(d.injuries) + 1)
    )
    .stop()

  $: for (let i = 0; i < 1000; ++i) simulation.tick()

  $: render = ({ context }) => {
    context.fillStyle = "#657fc9"
    context.fillRect(331, 88, 45, 17)

    context.fillStyle = "white"
    context.beginPath()
    context.font = "14px Alegreya Sans"
    context.fillText("2000", 340, 100)
    context.fill()

    context.fillStyle = "#657fc9"
    context.fillRect(331, 1088, 45, 17)

    context.fillStyle = "white"
    context.fillText("2005", 340, 1100)
    context.fill()
  }
</script>

<Canvas {width} {height}>
  {#each data as d}
    <Point
      x={d.x}
      y={d.y}
      r={radiusScale(d.injuries)}
      fill={d.injuries < 20 ? "#FCFCFC" : "#e6cea1"}
    />
  {/each}
  <Layer {render} />
</Canvas>
