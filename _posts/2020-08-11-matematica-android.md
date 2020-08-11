---
title: Matemática, uma ferramenta do cinto de utilidades do Desenvolvedor Android
tags: [Android, Matemática]
style: 
color: 
description: Utilizando a matemática como uma ferramenta no seu dia a dia de programador
---
![](https://cdn-images-1.medium.com/max/2560/1*DW6fDsUZRmlM_QymBY4AQA.jpeg)


Muito provavelmente em algum momento de sua vida escolar  você já escutou ou falou o seguinte questionamento: “Porque eu tenho que aprender isso se eu nunca vou usar na minha vida?”. Muitos assuntos foram vítimas desse pensamento e muito provavelmente o teorema de pitágoras é o campeão de reclamações.

> Em qualquer triângulo retângulo, o quadrado do comprimento da hipotenusa é igual à soma dos quadrados dos comprimentos dos catetos.

![](https://cdn-images-1.medium.com/max/800/1*eV4odP2jqjF2b66FMapuXQ.gif)

Uma aplicação do teorema é no cálculo da distância entre dois pontos em um plano cartesiano, onde um dos catetos será (x2 - x1) e o outro será (y2 - y1).

![](https://cdn-images-1.medium.com/max/800/1*se0ftrCe1q7CqDd8Keyqjg.png)

Recentemente tive mais uma oportunidade de tornar inválido o questionamento do início desse artigo, me deparei com o desafio de construir uma view customizada que exibe no momento do clique uma animação de gota que ficaria assim:

![](https://cdn-images-1.medium.com/max/800/1*Rie7ybsT2RDOFGRWqBRwfw.gif)

Para atingir tal comportamento no Android utilizaremos o método `ViewAnimationUtils.createCircularReveal()` e vamos precisar da view a ser animada, das coordenadas do clique na view, raio inicial e raio final da animação. 

![](https://cdn-images-1.medium.com/max/800/1*eLdxWB3Y1q8KJOw_cs__LA.png)

Para calcular o raio final vamos ter que calcular o cateto oposto(oppositeSide) e o cateto adjacente(adjacentSide). Para medir o cateto adjacente é preciso identificar se a coordenada x do clique foi maior ou igual a metade da largura(e usamos a mesma lógica para medir o cateto oposto).

![](https://cdn-images-1.medium.com/max/800/1*XL4tjKfv_WMld4b3xDexLQ.png)

Simulação de dois cliques diferentes.

Colocando em prática toda teoria discutida ficamos com um código assim:

```kotlin
import kotlin.math.hypot

val adjacentSide = if (touchX > width / 2) touchX else width - touchX
val oppositeSide = if (touchY > height / 2) touchY else height - touchY

val endRadious = hypot(adjacentSide, oppositeSide)
val startRadious = 0f

ViewAnimationUtils.createCircularReveal(
	installmentView,
	touchX.toInt(), 
	touchY.toInt(), 
	startRadious,
	endRadious
)
```

Definitivamente matemática é um assunto recorrente no desenvolvimento Android, afinal estamos construindo o tempo todo views customizadas que ou reaproveitam implementação de algum componente nativo ou criamos do zero desenhando no canvas. Por isso tenha a matemática como ferramenta auxiliar no seu dia a dia como desenvolvedor e deixe seu cinto de utilidades mais robusto.