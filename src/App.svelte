<script>
  import { csv } from "d3"
  import { onMount } from "svelte"
  import Map from "./../components/Map.svelte"
  import LineCounts from "./../components/LineCounts.svelte"
  import LineFatalities from "./../components/LineFatalities.svelte"
  import ForceFatalities from "./../components/ForceFatalities.svelte"
  import Timing from "./../components/Timing.svelte"
  import features from "./../public/data/dataGeoCountries2.js"
  let yearsCounts = []
  let yearsFatalities = []
  let dataUS = []
  let timing = []

  onMount(async () => {
    const countsRaw = await csv("data/years.csv", (d) => {
      return {
        year: +d.year,
        sum: +d.sum,
        isFatal: d.isFatal,
      }
    })
    yearsCounts = countsRaw

    const sumFatalities = await csv("data/yearsFatalities.csv", (d) => {
      return {
        year: +d.year,
        sum: +d.sum,
      }
    })
    yearsFatalities = sumFatalities

    const data = await csv("data/dataUSSelected.csv", (d) => {
      return {
        injuries: +d.injuries,
        year: +d.year,
      }
    })
    dataUS = data

    const dataTiming = await csv("data/timing.csv", (d) => {
      return {
        phase: d.phase,
        num: +d.num,
      }
    })
    timing = dataTiming
  })

  // $: console.log(yearsCounts)
</script>

<main>
  <h1>Anatomy of plane crashes</h1>
  <p>
    Back in 1996 two commercial airliners from Saudi Arabia and Kazakstan
    collided in the sky over New Delhi, killing at least 349 people in the
    deadliest plane crash in history.
  </p>
  <p>
    In more recent times, a Malaysian airliner with 295 people on board flying
    from Amsterdam to Kuala Lumpur crashed in the sky over eastern Ukraine.
  </p>
  <p>The third deadliest crash happened in the sky over the United Kingdom.</p>
  <Map {features} />
  <p>
    The good news is, large-scale airplane crashes leading to many fatalities
    are happenning less and less.
  </p>

  <p class="last">
    Most airplane crashes are <span class="yellow">not fatal</span> and are in decline.
  </p>

  <p class="title">
    Number of non-fatal and <span class="yellow">fatal</span> airplane crashes
  </p>
  <LineCounts data={yearsCounts} />
  <p class="title">Number of fatalities, by year</p>
  <LineFatalities data={yearsFatalities} />
  <p class="first">
    We all heard about large-scale airplane crashes making headlines in the last
    decades. The majority of fatal airplane crashes, however, are less
    sensational and happen with small planes at an astonishing monthly
    frequency.
  </p>
  <p>
    Let's look at every fatal crash in the last years in the United States. On
    the chart below, plane crashes are grouped by year and every circle is
    proportional to the number of people who died in the crash. Events in which
    more than 20 persons died are are highlighted in <span class="yellow"
      >yellow</span
    >.
  </p>
  <p class="last">
    Before 2000s there had been several large scale commercial aiplane crashes
    with many people on board. Such event are becoming increasingly rare.
    However, small private planes with a few people on board crash with almost
    daily frequency. Together, these events account for a larger number of
    fatalities that a few bigger crashes.
  </p>
  <ForceFatalities data={dataUS} />
  <p class="first last">
    Before 2000s, there had been several large-scale commercial airplanes
    crashes with many people on board. Such events are becoming increasingly
    rare. However, small private planes with a few people on board crash with
    almost daily frequency. Together, these events account for a more
    significant number of fatalities than a few bigger crashes.
  </p>
  <p class="title">Number of fatalities by timing during the flight</p>
  <Timing data={timing} />
</main>

<style>
  .yellow {
    color: #e6cea1;
    font-weight: bold;
  }
  h1,
  p {
    color: #fcfcfc;
  }

  .last {
    margin-bottom: 80px;
  }

  .first {
    margin-top: 80px;
  }

  h1 {
    margin-top: 60px;
    margin-bottom: 60px;
    text-align: center;
  }
  main {
    /* display: flex;
     align-items: center;
    justify-content: center;
    flex-direction: column; */
    width: 700px;
    margin: 0 auto;
  }
</style>
