---
title: "Açude, açude meu"
date: 2017-11-15T21:42:58-03:00
draft: false
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/vega/3.0.7/vega.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-lite/2.0.1/vega-lite.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-embed/3.0.0-rc7/vega-embed.js"></script>
<script>
    const maximasUltimaSangria = "https://gist.githubusercontent.com/JoseRenan/8f65eee8f0eeddc6d64b73e65cce4f02/raw/466ed8750475896be1e8919aa1fd50cfb582d01e/maximas-desde-ultima-sangria.json";

    const inicioRacionamento = "https://gist.githubusercontent.com/JoseRenan/8f65eee8f0eeddc6d64b73e65cce4f02/raw/3e5eba5b41f00d7ddad469a3eb17a4bd5115b160/inicio-racionamento.json";

    const comparacaoRacionamento = "https://gist.githubusercontent.com/JoseRenan/8f65eee8f0eeddc6d64b73e65cce4f02/raw/9059920352eddc64a6e08c035722c5ca93f9c415/comparacao-racionamento.json";

    const fimRacionamento = "https://gist.githubusercontent.com/JoseRenan/8f65eee8f0eeddc6d64b73e65cce4f02/raw/466ed8750475896be1e8919aa1fd50cfb582d01e/fim-racionamento.json";

  	vegaEmbed('#maximas', maximasUltimaSangria).catch(console.warn);
    vegaEmbed('#inicio-racionamento', inicioRacionamento).catch(console.warn);
    vegaEmbed('#comparacao-racionamento', comparacaoRacionamento).catch(console.warn);
    vegaEmbed('#fim-racionamento', fimRacionamento).catch(console.warn);
</script>

Temos visto ultimamente constantes discussões sobre se devemos ou não voltar com o racionamento na cidade de Campina Grande, a maior consumidora de água do açude de Boqueirão. Para contextualizar melhor, nosso açude vem perdendo volume desde 2011, quando ultrapassou sua capacidade máxima pela última vez. O gráfico a seguir mostra os maiores níveis que o açude atingiu em cada ano desde sua última sangria.

<div id="maximas"></div>

Em 2014 o açude chegou a menos da metade de sua capacidade, o que levou as entidades responsáveis a tomar a decisão de iniciar o racionamento. Como pode ser visto abaixo, o racionamento conseguiu diminuir a queda de nível do açude já desde a primeira metade de 2015.

<div id="inicio-racionamento"></div>

Passados os anos, antes tarde do que nunca, chega no início desse ano a transposição do Rio São Francisco à Boqueirão trazendo, junto com a água, esperança para todos os que viviam da água do açude. Depois de 4 anos de queda, os níveis de volume voltaram a subir como pode se ver no comparativo abaixo.

<div id="comparacao-racionamento"></div>

Esse aumento nos níveis do açude trouxe o debate de se devemos parar ou não o racionamento mesmo sem nem termos chegado aos 10% do volume. Para adiantar a história, na segunda metade do ano saímos do racionamento e ainda não sabemos se haverão consequencias futuras disso e isso divide opiniões até entre os técnicos da área. O gráfico abaixo mostra o crescimento do volume de água após a chegada da transposição, que decai um pouco no mês de setembro, que pode ter ocorrido pelo fim do racionamento, ou não. O que importa agora é que com ou sem racionamento temos de fazer nossa parte e nos conscientizar sobre o uso da água para que não voltemos situação que estávamos poucos meses atrás. 

<div id="fim-racionamento"></div>

Fonte dos dados: ANA e AESA.