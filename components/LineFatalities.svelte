<script>
  import {
    line,
    scaleLinear,
    extent,
    max,
    curveCatmullRom,
  } from "d3"

  export let data
  let width = 700
  let height = 500

  let marginLeft = 60
  let marginRight = 40

  $: years = [1982, 1990, 2000, 2010, 2020]

  $: extentX = extent(data, (d) => d.year)
  $: maxY = max(data, (d) => d.sum)

  $: yTicks = [200, 400, 600, 800, 1000, 1200, 1400]

  $: xScale = scaleLinear()
    .domain(extentX)
    .range([marginLeft, width - marginRight])

  $: yScale = scaleLinear()
    .domain([0, maxY])
    .range([height - 10, 10])

  $: path = line()
    .x((d) => xScale(d.year))
    .y((d) => yScale(d.sum))
    .curve(curveCatmullRom)

</script>

<svg {width} {height}>
  <path d={path(data)} fill="none" stroke="#fcfcfc" />
  {#each data as point}
    <circle
      cx={xScale(point.year)}
      cy={yScale(point.sum)}
      r="3"
      fill="#fcfcfc"
    />
  {/each}
  {#each years as year}
    <g class="tick-x" transform="translate({xScale(year) - 10}, {height - 10})">
      <text x="0" y="0" fill="#fcfcfc" opacity="0.5">{year}</text>
    </g>
  {/each}
  {#each yTicks as tick}
    <g class="tick-y" transform="translate({10}, {yScale(tick)})">
      <text x="0" y="0" fill="#fcfcfc" opacity="0.5">
        {tick}
      </text>
    </g>
  {/each}
</svg>

<style>
  svg {
    display: block;
  }
</style>
