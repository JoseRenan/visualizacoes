---
title: "Lab 2"
date: 2017-12-12T09:18:37-03:00
draft: true
---

<script src="https://d3js.org/d3.v4.min.js"></script>

<div class="container">
    <svg class="mychart1" id="chart1"></svg>
</div>

<div class="container">
    <svg class="mychart2" id="chart2"></svg>
</div>

<div class="container">
    <svg class="mychart3" id="chart3"></svg>
</div>

<script type="text/javascript">
    "use strict"

    function desenhaGrafico(data) {

        var margin = {top: 20, right: 20, bottom: 40, left: 50},
            width = 1000 - margin.left - margin.right,
            height = 350 - margin.top - margin.bottom;

        var parseTime = d3.timeParse("%H:%M");
        var converteLista = data.map(function(d) {
            return parseTime(d.key);
        })

        console.log(data);

        var x = d3.scaleTime()
            .domain([d3.min(d3.values(converteLista)), d3.max(d3.values(converteLista))])
            .rangeRound([0, width]);

        var y = d3.scaleLinear()
            .domain([0, d3.max(data, function(d) { return d.value; })])
            .range([height, 0]);
        
        var area = d3.area()
            .x(function(d) { return x(parseTime(d.key)); })
            .y0(height)
            .y1(function(d) { return y(d.value); });
        
        var svg = d3.select("#chart1")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
                .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

        svg.append("path")
            .datum(data)
            .attr("class", "area")
            .attr("d", area);

        svg.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + height + ")")
            .call(d3.axisBottom(x).tickFormat(d3.timeFormat("%H:%M")));

        svg.append("g")
            .attr("class", "y axis")
            .call(d3.axisLeft(y));
        
    }

    d3.csv('https://raw.githubusercontent.com/luizaugustomm/pessoas-no-acude/master/dados/processados/dados.csv', function (dados) {
        var data = d3.nest()
            .key(d => d.horario_inicial)
            .rollup(v => d3.mean(v, d => d.total_pedestres))
            .entries(dados);
        desenhaGrafico(data);
    });
</script>