

## Instruções para criação de um script para experimento auditivo na plataforma Github

1. Crie um documento com extensão **.js**  dentro de uma pasta intitulada **data_includes** no seu diretório do Github. Para isso vá em **create new file**, escreva o nome da pasta indicado, digite barra ( a inclinada para a direita) e escreva o nome do seu documento de extensão **.js** (exemplo:**data_includes/meu_script.js**). Não se esqueça de que não pode haver **espaços** nem **caracteres especiais** no nome do seu script. Clique em **Commit new file** no final da página.

2.  Clique no nome do documento apenas criado e depois clique no ícone de caneta/lápis para começar a editar o seu script. 
**Dica**: utilize a função de comentário (qualquer texto que seja precedido por duas barras, **//**) para deixar o seu script mas organizado e auxiliá-lo na hora de revisar algum erro possível. Comente o que você acha que o código que você escreveu está realizando, assim, quando o *debug* localizar um erro você saberá com mais facilidade como e onde consertá-lo.

3.  O Primeiro comando que deve ser escrito é `PennController.ResetPrefix(null);`. Esse comando é essencial para que o resto dos seus comandos funcione. Ele inativa os prefixos que acompanham as declarações de elementos do PennController, tornando a linguagem mais simples e limpa. Para mais informações sobre esse tópico acesse a página da documentação sobre [Reset Prefix](https://www.pcibex.net/wiki/penncontroller-resetprefix/)

4. O próximo comando a ser escrito é o `Sequence()` Esse comando define a sequência na qual as telas do seu experimento irão aparecer. Nesse ponto é interessante você já ter em mente quantas telas você irá precisar, e também em qual ordem elas aparecerão. No nosso caso, precisaremos de quatro telas: uma de boas-vindas na qual o participante preencha seus dados, uma com as intruções para o experimento, uma com o experimento em si, e uma com instruções finais e um agradecimento pela participação. Um exemplo do uso do `Sequence` seria:

- `Sequence("Participante", "Instrucoes", randomize("Experimento"), SendResults(), "Final");.`

- Antes da tela **Experimento** foi utilizado o comando `randomize()`, que aleatoriza os itens exibidos dentro de um experimento. Também foi adicionado um comando antes de **Final**, o `SendResults()`. Esse comando salva automaticamente os dados coletados antes de exibir a tela **Final**, garantindo que não se perca os resultados por uma confusão do participante.

5. Antes de começar a escrever o código principal pode ser útil declarar um cabeçalho, ou um *Header*. O comando `Header()"` é executado antes de todo *Trial* (ou tela) e é útil para estabelecer de antemão as características de alguns *Elements*, além de definir variáveis globais (variáveis que podem ser utilizadas ao longo de todo o comando). Iremos declarar em nosso exemplo a característica de três *Elements*. O comando `defaultText()`, `defaultInputText()` e `defaultButton()` pré-define respectivamente os *Elements* `newText()`, `newTextInput()` e `newButton()`. Os três elementos terão as mesmas duas características (ou os mesmos dois comandos declarados dentro deles): `.css()`, com o qual definiremos o tamanho da fonte; e `.print()`, comando utilizado para indicar que aquele elemento deve ser impresso na tela. A única excessão será o elemento `newButton()`, que além dos dois comandos, conterá também `.center()`, para centralizar o botão na tela, e `.wait()`, que interrompe o processamento do programa até que o usuário interaja com o botão. `.wait()` é essencial para o funcionamento do código pois sem ele o programa processaria todos os comandos até a última linha, imprimindo os elementos na tela somente por alguns milessegundos. Abaixo, segue o exemplo do `Header()`:
```javascript
Header(
         defaultText
                  .css("font-size","1.2em")
                  .print()
         ,
         defaultTextInput
                  .css("font-size","1.2em")
                  .print()
         ,
         defaultButton
                  .css("font-size","1.2em")
                  .center()
                  .print()
                  .wait()
         ,
         
)
```
- O tamanho da fonte está em uma medida muito utilizada dentro da programação, o *em*. Se quiser se informar mais sobre isso acesse o artigo da [Wikipedia](https://en.wikipedia.org/wiki/Em_(typography)) sobre a medida.
Observe também que até aqui, todos os comandos tinham sido demarcados por **ponto e vírgula**, mas que agora, dentro de outras estruturas maiores como o *Header*, e, mais para frente o *Trial*, os comandos serão delimitados somente por uma **vírgula**. Mas **atenção** não há necessidade de colocar vírgula no último comando declarado dentro dessas estruturas mencionadas, já que o final dele coincidirá com o final da estrutura.

6. O comando a seguir é um dos mais básicos na construção de um experimento no *PennController*, o `newTrial()`. Esse comando é o responsável por criar novas telas, dentro das quais *Elements* serão declarados. Os *Elements* podem ser caixas de textos, imagens, áudios, botões, etc. todos extremamente importantes na construção do *script*. Após digitar o comando e abrir o parêntese, escreva o nome da tela, entre aspas, que você já tinha definido no comando anterior (`Sequence()`). Exemplo:
```javascript
newTrial("Participante",
  
)
```
7. O primeiro *Trial* será para coleta de dados sobre o participante, assim dentro dele será necessário ter uma breve mensagem de boas vindas, caixas de texto nas quais o participante digitará informações como seu nome, e-mail, idade, etc. e uma caixa de seleção na qual estará disponível algumas opções de escolaridade. Além disso será preciso ter um botão que possa ser clicado quando os campos de informações forem preenchidos, levando o participante para a próxima tela. Dessa forma o primeiro elemento a ser utilizado será o `newText()`. como o nome sugere, é um elemento de texto, ou seja, sua função é criar um novo texto que será utilizado no experimento. É nele que você escreverá as frases que aparecerão na tela. Um exemplo de do seu uso seria:
```javascript
newText("<p>Por favor, escreva seu NOME COMPLETO na caixa abaixo</p>")
,
```
- O comando contém só o texto a ser impresso pois, em nosso cabeçalho, declaramos que todo *Element* `newText()` seria impresso (além de formata-lo com o tamanho de fonte ideal). Assim não é necessário adicionar mais nenhum comando para que funcione corretamente.
Observe que além do texto entre aspas há também `<p>` e `</p>`. Esses dois símbolos são usados em programação para indicar um parágrafo. `<p>` indica o ínicio do parágrafo enquanto `</p>` indica o fim.
 
7. O próximo *Element* a ser usado será `newTextInput()`, que terá uma estrutura muito semelhante a `newText()`. Entretanto, como nosso objetivo ao usar esse *Element* é o de receber os dados do participante, entre parênteses teremos, ao invés do conteúdo a ser exeibido, o nome dado ao elemento, que posteriormente será utilizado para recuperar os dados enviados pelo usuário. Ao escolher um nome, tenha o cuidado de manter em mente que o PCIbex não reconhece **diacríticos** e que ele faz diferença entre **maíusculas e minúsculas**. Exemplo da estrutura do comando:
```javascript
newTextInput("Nome")
,
```
- O *Element* `newButton()` terá a mesma sintaxe que `newTextInput()` nesse primeiro *Trial*. Contudo, nos próximos ele apresentará algumas pequenas mudanças.

8. O *Element* a seguir é bem diverso dos que utilizamos até agora. O comando `newDropDown()` cria uma caixa com uma lista vertical na qual os itens podem ser selecionados. Pelo fato do `newDropDown()` ser um *Element* assim como `newText()`, `newTextInput`, etc. ele também conterá `.css()` e `.print()`, mas em sua estrutura será declarado ainda dois outros comandos: `.add()`, que irá adicionar as opções selecionaveis; e `.log()`, que enviará as informações coletadas para o documento de resultados. Exemplo:
```javascript
newDropDown("Escolaridade", "Selecione sua escolaridade")
         .add("Médio completo", "Superior em curso", "Superior completo", "Pós-graduação")
         .css("font-size","1.2em")
         .print()
         .log()
,
``` 
- Observe que a primeira palavra entre aspas é o nome do *Element*, enquanto a segunda é o texto padrão que aparecerá antes ser selecionada alguma das opções.
 
 9. Os comandos utilizados até agora alteraram a estrutura do experimento, moldando ele de acordo com o que foi declarado no *script*. Mas pouco se foi falado sobre os dados coletados e a página de resultados. Os comandos a seguir tratam desse assunto. Todas as nossas caixas de texto receberam nomes, esses serão utilizados no *Element* `newVar()`, que declara uma nova varíavel. Dentro dele, será utilizado: `.global()`, que torna a variável global; e `.set(getTextInput())`, que atribui os dados coletados nas caixas de texto às variáveis criadas. Um exemplo de uso seria:
 ```javascript
 newVar("NOME")
        .global()
        .set( getTextInput("Nome") )
 ,
```
- Assim como outros *Elements*, variáveis também tem de ser nomeadas. Nesse caso, para diferenciar o nome atribuido à caixa de texto do nome da váriavel, esse último foi escrito todo em caixa alta.
Com esse comando final, podemos fechar o nosso primeiro *Trial* e partir para o próximo.

- Até agora essa deve ser a estrutura do *Trial* **Participante**:
```javascript
//Cria uma nova tela - Tela de coleta de dados do participante
newTrial("Participante",

         newText("<p>Bem-Vindos!</p>")
         ,
         newText("<p>Neste experimento, você vai ouvir uma frase e depois deve escolher a melhor opção de interpretação para ela.</p>")
         ,
         newText("<p>Por favor, escreva seu NOME COMPLETO na caixa abaixo.</p>")
         ,
//Cria uma caixa de texto nomedada "Nome" para receber o nome do participante  
         newTextInput("Nome")
         ,
         newText("<p>Por favor, escreva o seu E-MAIL na caixa abaixo.</p>")
         ,
         newTextInput("Email")
         ,
         newText("<p>Escreva sua IDADE na caixa a abaixo.</p>")
         ,
         newTextInput("Idade")
         ,
         newText("<p>Agora selecione sua ESCOLARIDADE na caixa abaixo e aperte o botão 'Iniciar' para começar </p>")
         , 
//Cria uma caixa com seletores nomeada "Escolaridade" para que o participante selecione sua escolaridade
         newDropDown("Escolaridade", "Selecione sua escolaridade")
                  .add("Médio completo", "Superior em curso", "Superior completo", "Pós-graduação")
                  .css("font-size","1.2em")
                  .print()
                  .log() //Envia para o arquivo "results" a opção selecionada pelo participante 
         ,
//Cria um botão nomeado "Iniciar"
         newButton("Iniciar")
         ,
//Cria uma nova variável chamada "NOME" que recebe o conteúdo da caixa de texto "Nome"
         newVar("NOME")
                  .global()
                  .set( getTextInput("Nome") )
         ,
         newVar("EMAIL")
                  .global()
                  .set( getTextInput("Email") )
          ,
         newVar("IDADE")
                  .global()
                  .set( getTextInput("Idade") )
         
)
```
10. Antes de prosseguir para a próxima tela, faremos uso do comando `.log()` novamente, entranto, de uma maneira um pouco diferente. Dessa vez, ao invés de declará-lo com os parênteses em branco iremos dar um nome ao `.log()` e utilizar `.getVar()` para recuperar o conteúdo atribuído às váriáveis criadas na etapa anterior. Assim estaremos enviando os dados contidos nas varíaveis, isto é, os dados inseridos pelo participante, para o documento de resultados. Segue o exemplo de uso do comando:
```javascript
.log( "NOME" , getVar("NOME") )
```
11. O próximo *Trial* será nomeado **Instrucoes** e será composto basicamente de uma série de `newText()` identicos aos utilizados nas etapas anteriores. Ele também terá um botão nomeado **Iniciar** com, a princípio, as mesmas configurações do botão anteriormente criado. No entanto, para termos um controle maior no que diz respeito aos tempos de reação do participante, utilizaremos o comando `.log()`, dentro do *Element* `newButton()`para saber o momento exato em que o usuário iniciou o experimento em si. Assim, nosso exemplo ficará como se segue:
```javascript
newButton("Iniciar")
         .log()
```        
12. O *Trial* a seguir será o mais crucial, já que é nele que desenvolveremos a estrutura principal do experimento. Diferentemente das telas que contruímos até agora, iremos iniciar com o comando `Template()`. Esse comando irá agir de uma forma semelhante ao comando `default`, no entanto, ao invés de definir previamente a característica de algum elemento, o `Template()` irá definir a estrutura de vários *Trial* cujo os dados serão processados a partir de uma tabela (para saber mais sobre a criação dessa tabela acesse a pasta **chunk_includes** nesse repositório e leia o documento [Instruções](https://github.com/julia-greco/minicursoPCibex/blob/master/chunk_includes/Instru%C3%A7%C3%B5es.md)). Assim iremos declarar o nosso novo *Trial*, nomeado **Experimento** dentro de `Template`. Exemplo de uso:
```javascript
Template("minha_tabela.csv",

         variable => newTrial( "Experimento",    
                     )
)
```
- O comando `Template()` irá receber então o nome da tabela na qual os estímulos do experimento estarão registrados. Ainda teremos a criação de uma função que apontará para todas as linhas da tabela específicada. Essa função no nosso exemplo recebeu o nome de `variable`, o mesmo nome que aparece no [tutorial](https://www.pcibex.net/wiki/00-overview/) fornecido no site do PCIbex, contudo você também a encontrará nomeada como `row` em algumas partes da documentação disponível no site.

13. Agora, dentro do novo *Trial* utilizaremos o comando `newAudio()`, que irá reproduzir os áudios indicados. Como `newAudio()` é um *Element* sonoro, ao invés de utilizar o comando `.print()` para exibi-lo utilizaremos o comando `.play()`. Exemplo:
```javascript
newAudio("AudioExperimento", variable.AudioExperimento)
            .play()
,
```        
- É novamente utilizado o comando `variable`, e dessa vez, ela apontará para todas as linhas da coluna **AudioExperimento** presente na tabela. Portanto tome **cuidado** para não escrever o nome errado da coluna na qual estão citados os seus áudios, caso contrário o programa não conseguirá encontrar os seus arquivos.

14. Para ilustrar ao participante que um áudio será tocado, é interessante exibir uma imagem que indique tal ação, como, por exemplo, a imagem de um auto-falante. Assim utilizaremos a seguir o comando `newImage()` para a exibição de imagens. Como os outros *Elements* vistos anteriormente, será necessário declarar o comando `.print()`, utilizaremos também o comando `.size()`, que determina o tamanho da imagem. Um exemplo do uso desse *Element*:
```javascript
newImage("alto_falante_icone.png")
            .size( 90 , 90 )
            .print()
,
```
15. Para que o participante seja levado para a próxima parte do experimento, na qual ele analisará duas sentenças, iremos utilizar novamente um botão, que terá mais uma modificação dentre as que vimos até agora. Além de adicionarmos o comando `.log()`, adicionaremos também o comando `.remove()`, que irá remover o botão da tela assim que o mesmo for clicado. Isso se faz necessário pois não iremos declarar outro *Trial* dentro do nosso `Template()`, e portanto, tudo que adicionarmos na tela permanecerá, a não ser que seja removido por meio desse comando. Exemplo:
```javascript
newButton("Próximo")
            .log()
            .remove()
,
```        
16. Como dito anteriormente, nesse *Trial* precisaremos remover tudo que for adicionado. Para remover a imagem que adicionamos utilizaremos o comando `getImage()` em conjunto com o comando `.remove()`, identificando a imagem criada anteriormente e a removendo. Um exemplo do uso dessa estrutura é:
```javascript
getImage("alto_falante_icone.png")
            .remove()
,
```        
17. Depois de ouvir o áudio, o participante irá ler duas sentenças, portanto iremos utilizar dois `newText()`, entretanto, ao invés de escrevermos o texto que será impresso diretamente no elemento, utilizaremos `variable` para retomar as sentenças presentes na tabela. Exemplo:
```javascript
newText("A",variable.SentencaA)
,
```        
- Note que demos um nome ao novo texto criado: **A**. Esse nome será utilizado no comando a seguir.

18. Ao imprimir os textos na tela, eles automaticamente se posicionam um em baixo do outro, o que até agora não se apresentou como um problema. Contudo, é interessante que o participante possa ler as duas sentenças lado a lado. Para isso utilizaremos o comando `newCanvas()`, que permite alterar a posição das sentenças na tela. Entre os parenteses do nosso `newCanvas()` iremos definir o tamanho total da nossa tela em **pixels**. Com o comando `.add()` em conjunto com o comando `getText()`, iremos recuperar os textos já criados e posicioná-los na tela utilizando medidas novamente em **pixels**. Segue o exemplo do uso desse *Element*:
```javascript
newCanvas( 1400 , 700 )
            .add( 50 , 100 , getText("A") )
            .add( 750 , 100 , getText("B") )
            .print() 
,
```
- A medida à esquerda corresponde à **largura** da tela, enquanto a medida à direita corresponde à **altura** da tela. Perceba que no posicionamento das nossa sentenças a altura é a mesma, enquanto a largura é diferente, porque queremos que as frases fiquem lado a lado, mas não em cima uma da outra. Repare também que aqui utilizamos o comando `.print()`, isso ocorre pois o `newCanvas()` é um *Element*, e como todo outro *Element* visual precisa ser impresso para que suas alterações se tornem visíveis.

19. O último comando novo que utilizaremos em nosso script é o `newSelector()`. Esse comando possibilita que o participante escolha uma das sentenças, tanto pelo _**mouse**_ quanto pelo **teclado**. Para que a seleção por *mouse* das sentenças possa ocorrer, dentro do `newSelector()` utilizaremos novamente o comando `.add()` em conjunto com `getText()` para retomar as sentenças exibidas. Já para a seleção por teclas será utilizado o comando `.keys()`, dentro do qual indicaremos entre aspas quais teclas serão as seletoras. Iremos declarar ainda os comandos `.wait()` e `.log()`. Exemplo:
```javascript
newSelector()
          .add( getText("A") , getText("B") )
          .keys("A","B")
          .log()
          .wait()
)
```
20. Após fechar o *Trial* mas antes de fechar o `Template()` adicionaremos os dois últimos comandos `.log()`. Nesse caso eles irão retomar outras duas colunas da tabela criada: **Item** e **Group**, portanto iremos utilizar `variable`:
```javascript
.log("Group", variable.Group)
```
- Depois dos comando você pode fechar o `Template()` com `);`.

- O seu *Trial* do experimento deve se parecer com esse:
```javascript
//Indica o uso da tabela "treino_script_auditivo.csv"
Template("tabela_script_auditivo.csv",
// "variable" vai automaticamente apontar para cada linha da tabela "tabela_script_auditivo.csv"
         variable => newTrial( "Experimento",
//"variable" aponta para todas as linhas da coluna "AudioExperimento" da tabela "tabela_script_auditivo.csv" e toca o audio referente a elas
                  newAudio("AudioExperimento", variable.AudioExperimento)
                           .play()
                  ,
//Exibe na tela a imagem "alto_falante_icone.png"
                  newImage("alto_falante_icone.png")
                           .size( 90 , 90 )
                           .print()
       
                  ,
//Cria um botão nomeado "Próximo", envia para o arquivo "results" a informação de quando ele foi pressionado e remove ele da tela
                  newButton("Próximo")
                           .log()
                           .remove()
                  ,
//Remove a imagem "alto_falante_icone.png" 
                   getImage("alto_falante_icone.png")
                           .remove()
                  ,
//Cria um novo texto nomeado "A" e "variable" aponta para todas as linhas da coluna "SentencaA" e imprime o texto presente nelas 
                   newText("A",variable.SentencaA)
                  ,
                   newText("B",variable.SentencaB)
                  ,
//Cria um canvas (uma caixa) e coloca os textos "A" e "B" um ao lado do outro
                  newCanvas( 1400 , 700 )
                           .add( 50 , 100 , getText("A") )
                           .add( 750 , 100 , getText("B") )
                           .print() 
                  ,
//Possibilita a seleção dos textos "A" e "B" através do mouse ou das teclas "A" e "B". Também envia para o arquivo "result" qual texto foi selecionado
                  newSelector()
                           .add( getText("A") , getText("B") )
                           .keys("A","B")
                           .log()
                           .wait()
                  )
         
//Envia para o arquivo "results" o conteúdo da coluna "Group" 
         .log("Group", variable.Group)
         .log("Item", variable.item)
);
```
21. Crie mais um *Trial* intituludado **Final** e coloque alguns `newText()` com agradecimentos pela participação. Não se esqueça também de adicionar um comando `.wait()` no último `newText()` criado, pois como os resultados já foram salvos (devido ao comando que colocamos no `Sentence()`) não a necessidade de colocar um botão aqui. Exemplo:
```javascript
newText("<p> Você receberá um e-mail com a sua declaração de participação.</p>")
         .wait()
```
22. Após testar o seu programa e corrigir os erros com a ajuda do *debug*, será necessário fazer dois ajustes finais para que seu experimento esteja pronto para distribuição. O primeiro deles será ajeitar a barra de progresso, que até esse momento não deverá estar se completando mesmo ao final do experimento. Para corrigir tal erro será necessário usar o comando: `.setOption("countsForProgressBar",false);`. Esse deverá ir ao final do experimento, após o seu último *Trial*.
O outro ajuste será desativar o *debug*, e, para isso utilizaremos o código: `PennController.DebugOff();`. Esse deverá ir bem no início do script, logo após o comando: `PennController.ResetPrefix(null);`.

Finalmente o seu experimento está pronto!

Espero que esse tutorial tenha contribuido na construção do seu script!

Muito obrigada por acessá-lo,

Júlia Greco.


