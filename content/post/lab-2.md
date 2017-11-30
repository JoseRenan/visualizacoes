---
title: "Visualização do índice pluviométrico mensal de boqueirão"
date: 2017-11-29T23:34:52-03:00
draft: true
---

<title>Barras simples</title>
<script src="https://d3js.org/d3.v4.min.js"></script>


<div class="container">
    <div class="mychart" id="chart"></div>
</div>

<style>
    .mychart rect {
        fill: steelblue;
    }

    .mychart rect:hover {
        fill: goldenrod;
    }

    .mychart text {
        font: 12px sans-serif;
        text-anchor: left;
    }
</style>

<script type="text/javascript">
    "use strict"

    function desenhaGrafico(dados) {
        var alturaSVG = 400,
            larguraSVG = 900;

        var margin = {
                top: 10,
                right: 20,
                bottom: 30,
                left: 45
            }, // para descolar a vis das bordas do grafico
            larguraVis = larguraSVG - margin.left - margin.right,
            alturaVis = alturaSVG - margin.top - margin.bottom;

        /*
         * Prepara onde adicionaremos a visualizacao
         */
        var grafico = d3.select('#chart') // cria elemento <svg> com um <g> dentro
            .append('svg')
            .attr('width', larguraVis + margin.left + margin.right)
            .attr('height', alturaVis + margin.top + margin.bottom)
            .append('g') // para entender o <g> vá em x03-detalhes-svg.html
            .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')');

        // === EDITE DAQUI ===
        /*
         * As escalas
         */

        let min90percentil = d3.min(dados, (d) => parseInt(d.noventa_percentil));
        let max90percentil = d3.max(dados, (d) => parseInt(d.noventa_percentil));

        let min10percentil = d3.min(dados, (d) => parseInt(d.dez_percentil));
        let max10percentil = d3.max(dados, (d) => parseInt(d.dez_percentil));

        console.log()
        var x = d3.scaleLinear()
            .domain([min90percentil, max90percentil])
            .range([0, larguraVis]);

        var y = d3.scaleLinear()
            .domain([min10percentil, max10percentil + 1])
            .rangeRound([alturaVis, 0]);

        let cor = d3.scaleLinear()
            .domain([d3.min(dados, (d) => d.mediana), d3.max(dados, (d) => d.mediana)])
            .range(["#8cb0ff", "#0050ff"])
            .clamp(true);

        // === ATÉ DAQUI ===

        /*
         * As marcas
         */
        grafico.selectAll('g')
            .data(dados)
            .enter()
            .append('circle')
            .attr('cx', d => x(d.noventa_percentil)) // usando a escala definida acima
            .attr('cy', d => y(d.dez_percentil))
            .attr('r', 10)
            .attr('fill', d => cor(d.mediana));

        /*
         * Os eixos
         */
        grafico.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + alturaVis + ")")
            .call(d3.axisBottom(x)); // magica do d3: gera eixo a partir da escala

        grafico.append('g')
            .attr('transform', 'translate(0,0)')
            .call(d3.axisLeft(y)) // gera eixo a partir da escala

        grafico.append("text")
            .attr("transform", "translate(-30," + (alturaVis + margin.top) / 2 + ") rotate(-90)")
            .text("10-percentil");

        grafico.append("text")
            .attr("transform", "translate(" + ((larguraVis)/2) + "," + (alturaVis + 30) + " )")
            .text("90-percentil");
    }

    d3.csv('/visualizacoes/post/static/boqueirao-por-mes.csv', function (dados) {
        desenhaGrafico(dados);
    });
</script>
