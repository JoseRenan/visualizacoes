---
title: "Caminhos de Campina"
date: 2017-12-12T09:18:37-03:00
draft: false
---

<script src="https://d3js.org/d3.v4.min.js"></script>

<style>
div.tooltip {
  position: absolute;
  text-align: center;
  width: 100px;
  height: 48px;
  padding: 10px;
  font: 12px sans-serif;
  background: lightsteelblue;
  border: 0px;
  border-radius: 8px;
  pointer-events: none;
}
</style>

Campina Grande possui alguns pontos turisticos e um dos mais conhecidos é o açude velho, que geralmente faz parte do caminho do trabalho dos campinenses. O gráfico abaixo aproxima a quantidade de pessoas que passam pelo açude velho diariamente.
<div class="container">
    <svg class="mychart3" id="chart3"></svg>
</div>

O açude velho também é o lugar preferido de muita gente que tem o hábito de fazer caminhadas. O gráfico abaixo mostra quantas pessoas por momento do dia estão a pé passando pelo açude velho.

Selecione nos campos abaixo o range de tempo que deseja analisar:

<div class="container">
    <select id="select1">
        <option>Selecione o inicio do range</option>
    </select>
    <select id="select2">
        <option>Selecione o fim do range</option>
    </select>
    <button id="button1" onclick="updateChart1()">Atualizar gráfico</button>
    <div class="mychart3" id="chart1"></div>
</div>

E como poder esquecer dos nossos PETs? O gráfico abaixo mostra os pontos mais visitados por pessoas acompanhadas dos seus cachorros. Sendo o verde o Bob's, o laranja o monumento de jackson do pandeiro e o roxo o monumento dos burrinhos.

<div class="container">
    <svg class="mychart2" id="chart2"></svg>
</div>

<script type="text/javascript">
    "use strict"

    function desenhaGrafico1(data) {

        var parseTime = d3.timeParse("%H:%M");

        d3.select("#svgChart1").remove();
        d3.select("#chart1").append("svg").attr("id", "svgChart1");

        var sel1 = document.getElementById('select1');
        var valsel1 = sel1.options[sel1.selectedIndex].value;

        var sel2 = document.getElementById('select2');
        var valsel2 = sel2.options[sel2.selectedIndex].value;

        var newData = data.filter(d => parseTime(d.key) >= parseTime(valsel1) && parseTime(d.key) <= parseTime(valsel2));
        
        if (newData.length == 0) newData = data;

        var margin = {top: 20, right: 20, bottom: 40, left: 50},
            width = 700 - margin.left - margin.right,
            height = 350 - margin.top - margin.bottom;

        var converteLista = newData.map(function(d) {
            return parseTime(d.key);
        })

        console.log(newData);

        var x = d3.scaleTime()
            .domain([d3.min(d3.values(converteLista)), d3.max(d3.values(converteLista))])
            .rangeRound([0, width]);

        var y = d3.scaleLinear()
            .domain([0, d3.max(newData, function(d) { return d.value; })])
            .range([height, 0]);
        
        var area = d3.area()
            .x(function(d) { return x(parseTime(d.key)); })
            .y0(height)
            .y1(function(d) { return y(d.value); });

        var select1 = d3.select("#select1");
        data.forEach((e) => {
            select1.append("option").text(e.key).attr("value", e.key);
        });

        var select1 = d3.select("#select2");
        data.forEach((e) => {
            select1.append("option").text(e.key).attr("value", e.key);
        });

        var svg = d3.select("#svgChart1")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
                .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

        svg.append("path")
            .datum(newData)
            .attr("fill", "steelblue")
            .attr("stroke", "steelblue")
            .attr("stroke-linejoin", "round")
            .attr("stroke-linecap", "round")
            .attr("stroke-width", 1.5)
            .attr("d", area);

        svg.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + height + ")")
            .call(d3.axisBottom(x).tickFormat(d3.timeFormat("%H:%M")));

        svg.append("g")
            .attr("class", "y axis")
            .call(d3.axisLeft(y));
        
    }

    function desenhaGrafico2(data1raw, data2raw, data3raw) {

        var data1 = data1raw.values;
        var data2 = data2raw.values;
        var data3 = data3raw.values;

        var margin = {top: 20, right: 20, bottom: 40, left: 50},
            width = 700 - margin.left - margin.right,
            height = 350 - margin.top - margin.bottom;

        var parseTime = d3.timeParse("%H:%M");
        var converteLista = data1.map(function(d) {
            return parseTime(d.key);
        })

        var x = d3.scaleTime()
            .domain([d3.min(d3.values(converteLista)), d3.max(d3.values(converteLista))])
            .rangeRound([0, width]);

        var y = d3.scaleLinear()
            .domain([0, d3.max(data1, function(d) { return d.value; })])
            .range([height, 0]);
        
        var area = d3.line()
            .x(function(d) { return x(parseTime(d.key)); })
            //.y0(height)
            .y(function(d) { return y(d.value); });
        
        var svg = d3.select("#chart2")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
                .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

        svg.append("path")
            .datum(data1)
            .attr("fill", "none")
            .attr("stroke", "#1b9e77")
            .attr("stroke-linejoin", "round")
            .attr("stroke-linecap", "round")
            .attr("stroke-width", 1.5)
            .attr("d", area);

        svg.append("path")
            .datum(data2)
            .attr("fill", "none")
            .attr("stroke", "#d95f02")
            .attr("stroke-linejoin", "round")
            .attr("stroke-linecap", "round")
            .attr("stroke-width", 1.5)
            .attr("d", area);

        svg.append("path")
            .datum(data3)
            .attr("fill", "none")
            .attr("stroke", "#7570b3")
            .attr("stroke-linejoin", "round")
            .attr("stroke-linecap", "round")
            .attr("stroke-width", 1.5)
            .attr("d", area);

        svg.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + height + ")")
            .call(d3.axisBottom(x).tickFormat(d3.timeFormat("%H:%M")));

        svg.append("g")
            .attr("class", "y axis")
            .call(d3.axisLeft(y));
        
    }

    function desenhaGrafico3(dados) {
        var alturaSVG = 350, larguraSVG = 700;
        var	margin = {top: 10, right: 20, bottom:30, left: 45},
          larguraVis = larguraSVG - margin.left - margin.right,
          alturaVis = alturaSVG - margin.top - margin.bottom;

      /*
       * Prepara onde adicionaremos a visualizacao
       */
      var grafico = d3.select('#chart3')
          .attr('width', larguraVis + margin.left + margin.right)
          .attr('height', alturaVis + margin.top + margin.bottom)
        .append('g') // para entender o <g> vá em x03-detalhes-svg.html
          .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');

      // === EDITE DAQUI ===
      /*
       * As escalas
       */
      var x = d3.scaleBand()
	  	.padding(.1)
	  	.domain(dados.map((d) => d.nome))  // de um domínio de 1 a 5
	  	.rangeRound([0, larguraVis]); // para uma imagem de 0 a 10; // Configure essa escala com domain, range e padding

      var y = d3.scaleLinear()
	  	.domain([d3.max(dados, (d, i) => d.valor), 0])
		.rangeRound([0, alturaVis]);

      // === ATÉ DAQUI ===

      var div = d3.select("body").append("div")
        .attr("class", "tooltip")
        .style("opacity", 0);

      /*
       * As marcas
       */
      grafico.selectAll('g')
              .data(dados)
              .enter()
                .append('rect')
                  .attr('x', d => x(d.nome))   // usando a escala definida acima
                  .attr('width', x.bandwidth()) // largura da barra via escala
                  .attr('y', d => y(d.valor))
                  .attr('height', (d) => alturaVis - y(d.valor))
                    .on("mouseover", (d) => {
                        div.transition()
                            .duration(200)
                            .style("opacity", .9);
                        div.html(d.nome + "<br/>" + d.valor)
                            .style("left", (d3.event.pageX) + "px")
                            .style("top", (d3.event.pageY - 28) + "px");
                    })
                    .on("mouseout", function(d) {
                        div.transition()
                            .duration(500)
                            .style("opacity", 0);
                    });

      /*
       * Os eixos
       */
      grafico.append("g")
              .attr("class", "x axis")
              .attr("transform", "translate(0," + alturaVis + ")")
              .call(d3.axisBottom(x)); // magica do d3: gera eixo a partir da escala

      grafico.append('g')
              .attr('transform', 'translate(0,0)')
              .call(d3.axisLeft(y))  // gera eixo a partir da escala
    }

    d3.csv('https://raw.githubusercontent.com/luizaugustomm/pessoas-no-acude/master/dados/processados/dados.csv', function (dados) {
        var totalPedestresPor15Min = d3.nest()
            .key(d => d.horario_inicial)
            .rollup(v => d3.mean(v, d => d.total_pedestres))
            .entries(dados);

        desenhaGrafico1(totalPedestresPor15Min);    

        var totalCachorroLocal = d3.nest()
            .key(d => d.local)
            .key(d => d.horario_inicial)
            .rollup(v => d3.mean(v, d => d.pedestres_com_cachorro))
            .entries(dados);

        desenhaGrafico2(totalCachorroLocal[0], totalCachorroLocal[1], totalCachorroLocal[2]);
        desenhaGrafico3([
            {nome: "Motorizados", valor: d3.sum(dados, d => d.total_motorizados)},
            {nome: "Ciclistas", valor: d3.sum(dados, d => d.total_ciclistas)},
            {nome: "Pedestres", valor: d3.sum(dados, d => d.total_pedestres)}]);
    });

    function updateChart1 () {
        console.log("huehue");
        d3.csv('https://raw.githubusercontent.com/luizaugustomm/pessoas-no-acude/master/dados/processados/dados.csv', function (dados) {
            var totalPedestresPor15Min = d3.nest()
                .key(d => d.horario_inicial)
                .rollup(v => d3.mean(v, d => d.total_pedestres))
                .entries(dados);
            desenhaGrafico1(totalPedestresPor15Min); 
        });
    }
</script>