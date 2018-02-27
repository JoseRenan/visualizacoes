---
title: "Educacao no Brasil"
date: 2018-02-27T00:38:53-03:00
draft: false
---

<style>

.cidades {
  fill: none;
  stroke: #fff;
  stroke-linejoin: round;
}

path:hover, path.highlighted {
  fill: tomato;
}

div.tooltip {
  position: absolute;
  background-color: white;
  border: 1px solid black;
  color: black;
  font-family:"avenir next", Arial, sans-serif;
  padding: 4px 8px;
  display: none;
}

</style>

<svg width="1000" height="600"></svg>

<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/topojson.v2.min.js"></script>
<script src="/visualizacoes/post/static/legenda-d3-cor.js"></script>
<script>

var svg = d3.select("svg"),
    width = +svg.attr("width"),
    height = +svg.attr("height");

var path = d3.geoPath();

// a escala de cores
var color = d3.scaleThreshold().domain([20, 40, 60, 80, 100]).range(d3.schemeOranges[6]);

// função aux definida em legenda-d3-cor.js
desenhaLegenda(0, 100, color, "Professores do ensino básico com graduação em 2016 (%)")

d3.queue()
    .defer(d3.json, "/visualizacoes/post/static/br-tracts-topo.json")
    .await(ready);

function ready(error, dados) {
  if (error) throw error;

   var cidades = topojson.feature(dados, dados.objects.tracts).features;

  svg.append("g")
      .attr("class", "cidades")
    .selectAll("path")
    .data(cidades)
    .enter()
    .append("path")
      .attr("fill", d => {valor = d.properties.porcent2016; return valor === "NA" ? '#e0e0eb' : color(valor)})
      .attr("d", path)
      .on("mouseover",showTooltip)
      .on("mousemove",moveTooltip)
      .on("mouseout",hideTooltip)
}

// ZOOM

//create zoom handler
var zoom_handler = d3.zoom()
    .on("zoom", zoom_actions);

//specify what to do when zoom event listener is triggered
function zoom_actions(){
 d3.selectAll("path").attr("transform", d3.event.transform);
}

//add zoom behaviour to the svg element
//same as svg.call(zoom_handler);
zoom_handler(svg);


// TOOLTIP

//Create a tooltip, hidden at the start
var tooltip = d3.select("body").append("div").attr("class","tooltip");
//Position of the tooltip relative to the cursor
var tooltipOffset = {x: 5, y: -25};

function showTooltip(d) {
  moveTooltip();

  tooltip.style("display","block")
      .text(d.properties.cidade + ": " + d.properties.porcent2016 + "%");
}

//Move the tooltip to track the mouse
function moveTooltip() {
  tooltip.style("top",(d3.event.pageY+tooltipOffset.y)+"px")
      .style("left",(d3.event.pageX+tooltipOffset.x)+"px");
}

//Create a tooltip, hidden at the start
function hideTooltip() {
  tooltip.style("display","none");
}

</script>