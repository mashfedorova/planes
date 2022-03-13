<script>
  import { geoPath, geoEqualEarth } from "d3-geo"
  export let features
  let width = 700
  let height = width * 0.7
  const sphere = { type: "Sphere" }
  let projection = geoEqualEarth().fitSize([width, height], sphere)
  let path = geoPath(projection)
</script>

<svg {width} height="600">
  <g transform="translate(0,0)">
    {#each features as featureBase, i}
      <path
        d={path(featureBase)}
        stroke="none"
        stroke-width="2"
        fill="#bec7e0"
      />
    {/each}
  </g>

  <g transform="translate(0,0)">
    {#each features.filter((d) => d.iso3 === "GBR" || d.iso3 === "UKR" || d.iso3 === "IND") as featureBase, i}
      <path
        d={path(featureBase)}
        stroke="none"
        stroke-width="2"
        fill="#e6cea1"
      />
    {/each}
  </g>
</svg>

<style>
</style>
