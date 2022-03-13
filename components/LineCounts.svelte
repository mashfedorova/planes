<script>
  import {
    line,
    curveStep,
    scaleLinear,
    scalePoint,
    extent,
    scaleTime,
    max,
    range,
    min,
    curveNatural,
    curveCatmullRom,
  } from "d3"

  export let data
  let width = 700
  let height = 500

  let marginLeft = 60
  let marginRight = 40

  $: years = [1982, 1990, 2000, 2010, 2020]

  $: dataF = data.filter((d) => d.isFatal === "Fatal")
  $: dataNF = data.filter((d) => d.isFatal === "Non-Fatal")

  $: extentX = extent(data, (d) => d.year)
  $: maxY = max(data, (d) => d.sum)

  $: yTicks = [500, 1000, 1500, 2000, 2500]

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

  // $: console.log(maxY)

  // $: console.log(data.filter((d) => d.sum === 2707))
  // $: console.log(xScale(2000), xScale(2001), xScale(2002))
</script>

<svg {width} {height}>
  <path d={path(dataF)} fill="none" stroke="#e6cea1" />
  <path d={path(dataNF)} fill="none" stroke="#fcfcfc" />
  {#each dataF as point}
    <circle
      cx={xScale(point.year)}
      cy={yScale(point.sum)}
      r="3"
      fill="#e6cea1"
    />
  {/each}
  {#each dataNF as point}
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
    margin-bottom: 90px;
  }
</style>
