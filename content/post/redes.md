---
title: "Alô São Luísss!!!"
date: 2018-03-12T23:27:09-03:00
draft: false
---

<style>
        .node {
            fill: #ccc;
            stroke: #fff;
            stroke-width: 2px;
        }

        .link {
                stroke: #999;
                stroke-opacity: 0.3;
        }
</style>


Abaixo vemos um grafo de bandas e artistas relacionados à banda Tropykália, surgida no ano de 1997 com o sucesso "Não dá prazer" que a levou para vários estados do país. Seu primeiro DVD foi em 2005 em São Luís - MA com um público de mais de 50 mil pessoas e a incrível abertura que inicia com um grito que intitula este post: "Alô São Luís! Vem cantar com a gente nesse planeta de cores", fazendo referencia a outro grande sucesso que imortalizou a banda de forró.

Ao analisar o grafo, podemos perceber que grandes artistas do forró conteporâneo e sertanejo estão incluídas, como Simone e Simaria, Aviões do Forró, Gabriel Diniz, mostrando que apesar do tempo, o gênero continua vivo, se renovando a cada dia e fazendo sucesso em todo o país.


<div id="chart"></div>

<!-- scripts -->
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>
    var
        width = 1000,
        height = 1000;

    var svg = d3.select("#chart")
            .append("svg")
            .attr('version', '1.1')
            .attr('viewBox', '0 0 '+width+' '+height)
            .attr('width', '100%');

    var color = d3.scaleOrdinal(d3.schemeCategory20);

    var simulation = d3.forceSimulation()
        .force("link", d3.forceLink().id(function(d) { return d.id; }))
        .force("charge", d3.forceManyBody())
        .force("center", d3.forceCenter(width / 1.5, height / 4));

    d3.json("/visualizacoes/post/static/artistas-semelhantes.json", function(error, graph) {
        if (error) throw error;

        let nodes = graph.nodes.filter(d => d.size > 20).reverse();
        let edges = graph.edges.filter(d => nodes.filter(n => n.id === d.source).length > 0);
        edges = edges.filter(d => nodes.filter(n => n.id === d.target).length > 0);

        console.dir(graph.edges);
        console.dir(graph.nodes);

        var link = svg.append("g")
            .attr("class", "link")
        .selectAll("line")
            .data(graph.edges)
        .enter().append("line");

        var node = svg.append("g")
            .attr("class", "nodes")
        .selectAll("circle")
            .data(graph.nodes)
        .enter().append("circle")
            .attr("r", function(d) { return d.size / 10; })
            .attr("fill", function(d) { return color(d.group); })
            .call(d3.drag()
                .on("start", dragstarted)
                .on("drag", dragged)
                .on("end", dragended));

        node.append("title")
            .text(function(d) { return d.label; });

        simulation
            .nodes(graph.nodes)
            .on("tick", ticked);

        simulation.force("link")
            .links(graph.edges);

    var zoom_handler = d3.zoom()
    .on("zoom", zoom_actions);

    zoom_handler(svg);     

    function zoom_actions(){
        d3.selectAll("circle").attr("transform", d3.event.transform);
        d3.selectAll("line").attr("transform", d3.event.transform);
    }

        function ticked() {
        link
            .attr("x1", function(d) { return d.source.x; })
            .attr("y1", function(d) { return d.source.y; })
            .attr("x2", function(d) { return d.target.x; })
            .attr("y2", function(d) { return d.target.y; });

        node
            .attr("cx", function(d) { return d.x; })
            .attr("cy", function(d) { return d.y; });
        }
    });

    function dragstarted(d) {
        if (!d3.event.active) simulation.alphaTarget(0.3).restart();
        d.fx = d.x;
        d.fy = d.y;
    }

    function dragged(d) {
        d.fx = d3.event.x;
        d.fy = d3.event.y;
    }

    function dragended(d) {
        if (!d3.event.active) simulation.alphaTarget(0);
        d.fx = null;
        d.fy = null;
    }

</script>