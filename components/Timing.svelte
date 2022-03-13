<script>
  import { max } from "d3"

  import { scaleLinear } from "d3-scale"

  export let data

  let width = 700
  let height = 200

  const xTicks = [
    "landing",
    "takeoff",
    "cruise",
    "manoeuver",
    "approach",
    "climb",
    "descent",
    "taxi",
  ]

  const yTicks = [0, 5, 10, 15, 20, 25]

  const padding = { top: 20, right: 15, bottom: 20, left: 40 }

  $: xScale = scaleLinear()
    .domain([0, data.length])
    .range([padding.left, width - padding.right])

  $: yScale = scaleLinear()
    .domain([0, max(data, (d) => d.num)])
    .range([height - padding.bottom, padding.top])

  $: innerWidth = width - (padding.left + padding.right)
  $: barWidth = innerWidth / data.length
</script>

<svg class="drawing" data-name="Layer 1" viewBox="0 0 1280 350">
  <path
    d="M20,265H83.79c24,0,47.89-8,71.19-24L313.33,132.88C485.13,15.53,664.6,18.49,835.9,141.49L975.69,241.87c21.39,15.36,43.38,23.13,65.47,23.13H1130"
    fill="none"
    stroke="#e8e8e8"
    stroke-miterlimit="10"
    stroke-width="4"
    opacity=".61"
  />
  <path
    d="M335.21,105.51l-16.42-1.07a5,5,0,0,0-2.82.7l-2,1.17c-2.89,1.7-3.06,5.31-.31,6.43l8.72,3.55"
    fill="#a3b8e5"
  />
  <path
    d="M335.24,106.8l-16.43-1.07a4.9,4.9,0,0,0-2.81.69l-2,1.17c-2.89,1.7-3.06,5.31-.3,6.43l8.71,3.55"
    fill="#8c8c8c"
  />
  <path
    d="M302.61,138.29c4.46,7.14,47.83-10.58,56.62-15.74s12.29-15.14,7.83-22.28-15.21-8.75-24-3.58S298.14,131.14,302.61,138.29Z"
    fill="#c6c6c6"
  />
  <path
    d="M302.7,134.43l-6.3-6.57c-1.29-1.35-.73-3.52,1.19-4.57h0a3.81,3.81,0,0,1,4.42.39l7.93,7.87"
    fill="#a8a8a8"
  />
  <path
    d="M305.38,137.57l-.52,4.87c-.28,2.65,2.5,4.32,5.36,3.21h0c2.1-.81,3.41-2.84,3-4.68l-1.14-5.41L311,134.4"
    fill="#898e96"
  />
  <path
    d="M349.56,121.66l-.95,13.06c-.16,2.21-2.13,4.51-4.75,5.53l-3.28,1.29c-3.39,1.33-6.51,0-6.63-2.74l-.42-9.57a1.46,1.46,0,0,1,.87-1.24L348,121C348.78,120.61,349.62,120.93,349.56,121.66Z"
    fill="#a3b8e5"
  />
  <path
    d="M348.58,120.44l-.95,13.06c-.17,2.21-2.13,4.51-4.75,5.53l-3.28,1.29c-3.4,1.33-6.51,0-6.64-2.74l-.42-9.57a1.48,1.48,0,0,1,.87-1.24l13.56-7C347.8,119.39,348.64,119.71,348.58,120.44Z"
    fill="#777"
  />
  <path
    d="M365.91,99c-4.64,4.19-13.1,10.28-22.92,10.18-8.46-.09-9.93-2.78-9.62-5.1a107.33,107.33,0,0,1,9.8-7C351.22,92.35,360.91,93.31,365.91,99Z"
    fill="#c3cad8"
    stroke="#959596"
    stroke-miterlimit="10"
  />
</svg>

<svg width="600" height="300">
  <g class="axis y-axis">
    {#each yTicks as tick}
      <g transform="translate(0, {yScale(tick)})">
        <text y="0" fill="#fcfcfc" opacity="0.5">{tick}</text>
      </g>
    {/each}
  </g>

  <g class="axis x-axis">
    {#each xTicks as tick, i}
      <g transform="translate({xScale(i)},{height})">
        <text x={barWidth / 8} y="0" fill="#fcfcfc" opacity="0.5">{tick}</text>
      </g>
    {/each}
  </g>

  {#each data as point, i}
    <rect
      x={xScale(i) + 2}
      y={yScale(point.num)}
      width={barWidth - 4}
      height={yScale(0) - yScale(point.num)}
      fill="#fcfcfc"
    />
  {/each}
</svg>

<style>
  .drawing {
    margin-top: 50px;
  }
</style>
