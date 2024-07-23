<script>
  import  Canvas  from './components/Canvas.svelte';
  import data from "./data/berries.json";
  import { scaleLinear, scaleSequential} from "d3-scale";
  import {interpolatePiYG, interpolateRdBu} from "d3-scale-chromatic";
  import { extent, sort, descending, ascending} from "d3-array";
  import Circle from "./components/Circle.svelte";
  import Arrow from './components/Arrow.svelte';
  import Text from './components/Text.svelte';
  import Steps from "/components/Steps.svelte";
  import EmptyCircle from './components/emptyCircle.svelte';

  const margin ={
    top: 20,
    right: 20,
    bottom: 20,
    left:20
  }

  $: width = 355;
  $: innerWidth = width - margin.left - margin.right;

  let height = 474;
  let innerHeight = height - margin.top - margin.bottom;
  
  $: extentX = extent(data, (d) => d.Yield);
  $: extentY = extent(data, (d) => d.Flavor);
  $: extentColor = extent(data, (d) => d.Market_Condition);

  $: xScale = scaleLinear()
    .domain([0, extentX[1]])
    .range([0,innerWidth]);
  
  $: yScale = scaleLinear()
    .domain([0, extentY[1]])
    .range([innerHeight, 0]);

    $: colorScale = scaleSequential()
    .domain([extentColor[0],extentColor[1]])
    .interpolator(interpolatePiYG);

    let renderedData = sort(data, (a, b) => descending(a.Market_Condition, b.Market_Condition))

    let currentStep;

    let axisSymbol = false;
    let callOut = false;

    $: {
    if(currentStep === 0){
      axisSymbol = false;
      callOut = false;
    }
    else if(currentStep === 1){
      axisSymbol = true;
      callOut = false;
    }
    else if(currentStep === 2){
      axisSymbol = true;
      callOut = true;
    }
    else if(currentStep === 3){
      axisSymbol = true;
      callOut = true;
    }
  }
</script>

<main>
  <section>
  <div class="sticky">
	<Canvas
		width={width}
		height={height}
	>
		{#each renderedData as { id, Flavor, Market_Condition, Yield }}
			<Circle
				x={xScale(Yield)}
				y={yScale(Flavor)}
				r={3}
				fill={colorScale(Market_Condition)}
			/>
		{/each}
    <!-- {#if currentStep != 0}
      <Arrow 
       fromx={innerWidth / 2 - 10}
       fromy={innerHeight + margin.top + 5}
       tox={innerWidth / 2}
       toy={innerHeight + margin.top + 5}
       arrowWidth={3}
       color={"#666"}
      />
      <Arrow 
      fromx={10}
      fromy={innerHeight / 2 + 10}
      tox={10}
      toy={innerHeight / 2}
      arrowWidth={3}
      color={"#666"}
     />
     <Text
     font='18px RetinaNarrowLight'
     textAlign = 'center'
     textBaseline = 'middle'
     color='#666'
     text='Yield'
     text_x={innerWidth / 2 - 2 * margin.right}
     text_y={innerHeight + margin.top + 5}
     />
     <Text
     font='18px RetinaNarrowLight'
     textAlign = 'start'
     textBaseline = 'middle'
     color='#666'
     text='Flavor'
     text_x={0}
     text_y={innerHeight / 2 + margin.top + 12}
     />
    {/if}
    {#if currentStep > 1}
     <EmptyCircle
     x={innerWidth / 6 + 2 * margin.left}
     y={innerHeight / 6 - 2 * margin.top}
     r={30}
   />
   <EmptyCircle
   x={innerWidth * 4 / 5}
   y={innerHeight / 2 - margin.top}
   r={30}
 />
   {/if} -->
  </Canvas>
  </div>
  <Steps bind:currentStep/>
  </section>
</main>

<style>
  section {
  position: relative;
}
	main{
  max-width: 700px;
  margin:0 auto;
}
.sticky {
  position: sticky;
  z-index: 1;
  height:90vh;
  top:5vh;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  /* the three lines above is how a parent center its children */
}
</style>