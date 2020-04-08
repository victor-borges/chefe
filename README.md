# Chefe

## Introdução

Este projeto foi criado como trabalho da matéria de compiladores.
Chefe é uma linguagem de programação esotérica, onde os programas se parecem com receitas de culinária.
Criada originalmente por David Morgan-Mar (disponível em <http://www.dangermouse.net/esoteric/chef.html>).
A sintaxe e a especificação da linguagem foram traduzidas para o português.

## Princípios de _design_

- Receitas de programas devem almejar não apenas a compilação, mas também serem fáceis de preparar e deliciosas.
- Receitas usam o sistema métrico, mas podem usar medidas tradicionais da culinária como colheres e xícaras.

## Conceitos da linguagem

### Ingredientes

Todas as receitas possuem ingredientes! Ingredientes armazenam valores individuais. Ingredientes líquidos serão interpretados como caracteres Unicode, enquanto ingredientes secos ou não especificados serão interpretados como números.

### Tigelas e assadeiras

Chefe tem acesso a um estoque ilimitado de tigelas e assadeiras. Estas podem conter ingredientes. Ingredientes numa tigela ou numa assadeira são ordenados como uma pilha de panquecas. Novos ingredientes são inseridos no topo, e se ingredientes são removidos, serão também removidos do topo. Note que se o valor de um ingrediente muda, o valor na tigela ou assadeira continua igual.

Múltiplas tigelas ou assadeiras são referencias com um identificador ordinal (`a 2ª tigela`, por exemplo). Se nenhum identificador é utilizado, então a receita só possui um deste utensílio. Identificadores ordinais devem ser seguidos de uma terminação, `ª` ou `º`, para indicar o gênero.

### Elementos sintáticos

Os itens a seguir aparecem numa receita de Chefe. Alguns são opcionais. Itens devem aparecer na ordem mostrada abaixo, com uma linha em branco (duas quebras de linha) entre itens.

#### Título da receita

O título da receita descreve, em poucas palavras, o que o programa faz. Por exemplo: "Suflê de Olá Mundo" ou "Números Fibonacci com Caramelo". O título é sempre a primeira linha de uma receita, seguida de um ponto final.

<pre>
<i>titulo-da-receita</i>.
</pre>

#### Comentários

Comentários são constituídos de um parágrafo após o título da receita. Comentários são opcionais.

#### Lista de ingredientes

Lista os ingredientes que serão usados pelo programa. A sintaxe é descrita abaixo.

<pre>
Ingredientes.
<i>[valor-inicial] [medida [tipo-da-medida]] nome-do-ingrediente
[valor-inicial] [medida [tipo-da-medida]] nome-do-ingrediente</i>
</pre>

Cada ingrediente é listado em uma linha. O valor inicial é um número, e é opcional. Tentar utilizar um ingrediente sem um valor definido é um erro de tempo de execução. A `medida` pode ser qualquer uma das seguintes medidas:

- `g | kg | pitada[s]`: Indicam ingredientes sólidos.
- `ml | l | fio[s]`: In ingredientes líquidos.
- `colher[es] [de [sopa | chá]] | xícara[s] | copo[s]`: Indicam tanto ingredientes sólidos como líquidos.

O nome do ingrediente pode ser qualquer frase, e pode incluir espaços em branco. A lista de ingredientes é opcional. Se um ingrediente é repetido, o novo valor é usado e os valores antigos são descartados.

#### Tempo de preparo

<pre>
Tempo de preparo: <i>tempo</i> (hora[s] | minuto[s]).
</pre>

O tempo de preparo é opcional. O `tempo` é um número.

#### Temperatura do forno

<pre>
Pré-aqueça o forno a <i>temperatura</i> °C.
</pre>

Algumas receitas requerem aquecimento. A temperatura do forno é opcional.

#### Modo de preparo

<pre>
Modo de preparo.
<i>métodos de preparação</i>
</pre>

O modo de preparo contém as instruções para a receita. Estas são escritas em sentenças. Quebras de linha são ignoradas no modo de preparo de uma receita. Instruções válidas são:

- `Retire [o|a]`_`ingrediente`_`do refrigerador.`
  Lê um valor do `STDIN` para o _ingrediente_, sobrescrevendo qualquer valor anterior.

- `Coloque [o|a]`_`ingrediente`_`na [iª] tigela.`
  Coloca um _ingrediente_ na _iª_ tigela.

- `Sove [o|a]`_`ingrediente`_`na [iª] tigela.`
  Remove o valor do topo da _iª tigela_ e o coloca no _ingrediente_.

- `Adicione [o|a]`_`ingrediente`_`[na [iª] tigela].`
  Adicona o valor de _ingrediente_ ao valor do ingrediente no topo da _iª tigela_, e guarda o resultado na _iª tigela_.

- `Remova [o|a]`_`ingrediente`_`[da [iª] tigela].`
  Subtrai o valor de _ingrediente_ do valor do ingrediente no topo da _iª tigela_, e guarda o resultado na _iª tigela_.

- `Combine [o|a]`_`ingrediente`_`[na [iª] tigela].`
  Multiplica o valor de _ingrediente_ pelo valor do ingrediente no topo da _iª tigela_, e guarda o resultado na _iª tigela_.

- `Divida [o|a]`_`ingrediente`_`[na [iª] tigela].`
  Divida o valor de _ingrediente_ pelo valor do ingrediente no topo da _iª tigela_, e guarda o resultado na _iª tigela_.

- `Adicione os ingredientes sólidos [na [iª] tigela].`
  Adiciona o valor de todos os ingredientes sólidos e guarda o resultado na _iª tigela_.

- `Liquidifique [o|a]`_`ingredientes.`_
  Torna o ingrediente em líquido, ou seja, um caractere Unicode para saída de dados.

- `Liquidifique o conteúdo da [iª] tigela.`
  Torna todos os ingredientes da _iª tigela_ em líquido, ou seja, em caracteres Unicode para saída de dados.

- `Misture [a [iª] tigela] por`_`número`_`minutos.`
  Empurra o primeiro ingredientes na _iª tigela_, tal que o ingrediente no topo vai _número_ de lugares pra baixo e todos os outros ingredientes no caminho são mandados um lugar pra cima. Se não há este número de ingredientes na tigela, o ingrediente no topo vai para a posição mais abaixo, e todos os outros sobem um lugar.

- `Misture [o|a]`_`ingrediente`_`na [iª] tigela.`
  Empurra o primeiro ingredientes na _iª tigela_, tal que o ingrediente no topo desde o valor de _ingrediente_ lugares pra baixo e todos os outros ingredientes no caminho são mandados um lugar pra cima. Se não há este número de ingredientes na tigela, o ingrediente no topo vai para a posição mais abaixo, e todos os outros sobem um lugar.

- `Misture bem [a [iª] tigela].`
  Randomiza a ordem dos ingredientes na _iª_ tigela.

- `Limpe a [iª] tigela.`
  Remove todos os ingredientes da _iª_ tigela.

- `Despeje o conteúdo da [iª] tigela na [jª] assadeira.`
  Copia todos os ingredientes da _iª_ tigela para a _jª_ assadeira, mantendo a ordem os colocando em cima de qualquer coisa que já esteja na assadeira.

- _`Verbo`_`[o|a]`_`ingrediente.`_
  Marca o começo de um _loop_. Deve aparecer como um par com a declaração abaixo. O _loop_ é executado desta forma: o valor do _ingrediente_ é checado. Se é diferente de zero, o corpo do _loop_ é executado até chegar na declaração _até_. O valor do ingrediente é checado novamente. Se é diferente de zero, o _loop_ executa de novo. Se em qualquer checagem o valor do ingrediente for zero, o _loop_ para e a execução continua na declaração depois do _até_. _Loops_ podem ser aninhados.

- _`Verbo`_`[[o|a] ingrediente] até`_`frase.`_
  Marca o fim de um _loop_. Deve aparecer como um par com a declaração acima. O _verbo_ nesta declaração é arbitrário e ignorado. Se o _ingrediente_ aparece nesta declaração, seu valor é decrementado por 1 quando esta declaração for executada. O _ingrediente_ não precisa ser o mesmo _ingrediente_ do começo do _loop_.

- `Deixe descansar.`
  Causa o fim prematuro e imediato do _loop_ mais interno em que esta declaração aparece e continua na declaração após o _até_.

- `Sirva com`_`receita-auxiliar.`_
  Invoca um sous-chef para imediatamente preparar a _receita-auxiliar_. O chefe que o chamou aguarda até que o sous-chef acabe para continuar a sua receita. Veja a seção de receitas auxiliares abaixo.

- `Refrigere [por`_`número`_`horas].`
  Causa o fim prematuro e imediato da receita em que essa declaração aparece. Se for uma receita auxiliar, a receita acaba e a primeira tigela do sous-chef é passada de volta para o chefe que o chamou, normalmente. Se o _número_ de horas for especificado, a receita vai imprimir as _número_ primeiras assadeiras (veja a seção Rendimento abaixo) antes de encerrar.

#### Rendimento

A última frase de uma receita é a quantidade de porções que ela rende, ou, a quantidade de pessoas que ela serve.

<pre>
Rendimento: <i>número-de-porções</i> (porções | pessoas).
</pre>

Este comando escreve para o `STDOUT` o conteúdo das primeiras `número-de-porções` assadeiras. Começa pela `1ª assadeira`, removendo os valores do topo um por um e os imprimindo, até que a assadeira fique vazia. Então, progride para a próxima assadeira, até que todas sejam impressas.

O rendimento é opcional numa receita, mas é necessário se o programa tiver qualquer saída!

#### Receitas auxiliares

Algumas receitas pequenas podem ser necessárias para produzir ingredientes especializados para a receita principal (como molhos, ou caldas). Eles são listados na receita principal. Receitas auxiliares são feitas por sous-chefs, então eles têm o próprio conjunto de tigelas e assadeiras que o Chefe principal nunca vê, mas pode copiar todas as tigelas e assadeiras em uso pelo chef que o chamou. Mudanças nessas tigelas e assadeiras não interferem nas tigelas e assadeiras do chef que o chamou. Quando a receita auxiliar termina, os ingredientes `na 1ª tigela` são passados para `a 1ª tigela` do chefe que o chamou.

Uma receita auxiliar pode ter todos os itens de uma receita principal.

#### Receitas de exemplo

- [Suflê de Olá Mundo](receitas/sufle-de-ola-mundo.chefe)
- [Números de Fibonacci com Caramelo](receitas/numeros-de-fibonacci-com-caramelo.chefe)
