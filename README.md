Repositório com exemplos do post do blog da Concrete (fora do ar)

# Testes web com Robot Framework para Python: Introdução

Antes de começar, gostaria de agradecer ao [Darling Lopes Cabral](https://www.linkedin.com/in/darling-cabral-6b709a49/) por me apresentar o Robot me ajudando com os primeiros passos, e a [Mayara Ribeiro Fernandes](https://www.linkedin.com/in/mayfernandes), por me ajudar indiretamente pelo blog [Robotizando Testes](http://robotizandotestes.blogspot.com.br/) e diretamente pelo LinkedIn.

Nesta introdução, vou dar uma visão geral sobre o que é o Robot e fazer um script simples de busca no Google.


## O que é o Robot Framework?

O [Robot](http://robotframework.org/) é um framework para automação de testes de aceitação e _acceptance test-driven development_ (ATDD). É _open source_, independente de sistema operacional, nativamente implementado para Python e Java. Também roda em Jython (JVM) e IronPython (.NET).

Sua sintaxe é tabular, como Python, e usa uma abordagem de palavras chave (_keyword driven_). Esta abordagem é que mais me chamou atenção no Robot, pois permite a escrita de cenários com uma linguagem totalmente natural. É possível também usar a sintaxe Gherkin.


## Estrutura do código

A estrutura do script é simples e pode ser dividida em 4 seções:


### *** Settings ***

Aqui é onde você configura as bibliotecas que serão utilizadas, caminhos para arquivos auxiliares (page objects por exemplo), contextos e hooks.


### *** Variables ***

Lista das variáveis a serem usadas (de preferência com descrição) e definição dos valores de algumas dessas variáveis.


### *** Test Cases ***

Esta é a seção mais importante, pois sem ela seu teste não roda. É aqui que ficam os cenários/casos de teste, com ou sem implementação.


### *** Keywords ***

Aqui você pode definir palavras chave ou implementar os cenários de teste descritos acima.

Todas as seções acima são opcionais, dependendo de como seu código foi escrito, exceto a *** Test Cases ***. Aconselho a usá-las sempre para uma melhor organização do código.

Os rótulos abaixo não são obrigatórios, mas também ajudam na organização.


### [Documentation]

Esta palavra chave precede a descrição da funcionalidade ou do cenário de teste. Tenha a atenção que, se a documentação é feita na seção *** Setings ***, ela não deve estar entre colchetes. Os colchetes só são usados dentro dos *** Test Cases ***


### [Tags]

Um rótulo mais simples para o cenário, caso queira ou precise rodar somente um ou poucos casos. É possível fazer isso chamando pelo título dos cenários na linha de comando mas, como geralmente eles são grandes, as tags são uma melhor opção em geral.


### Espaços e variáveis

As variáveis no Robot são representadas por `${variavel}`. O Robot tem a peculiaridade de ignorar um espaço entre palavras. `${nome_da_variavel}` é igual a `${nome da variavel}`. Elas também são _case insensitive_.

As divisões dos argumentos são feitas por no mínimo dois espaços. Por exemplo:


```
comando (dois espaços) argumento1 (dois espaços) argumento 2  …
```


Reparem que o espaço no “argumento 2” é ignorado. O sinal de atribuição (=) também é opcional. Valores podem ser atribuídos tanto


```
${valor}  valor 
${valor} =  valor
```


Essa regra de espaços é bem confusa no início, mas com a _sintaxe highlight_ você percebe se está cometendo algum erro. Outra opção é mostrar os caracteres invisíveis.


### Indentação

O Python é uma linguagem [indentada](https://pt.wikibooks.org/wiki/Python/Conceitos_b%C3%A1sicos/Indenta%C3%A7%C3%A3o), ou seja, blocos de comando são separados por espaços ou tabulações, formando uma indentação visual obrigatória. Não existem símbolos de “abre” e “fecha”. O Robot herda essa formatação.


## Instalação

O pré requisito é ter o [Python 2.7](https://www.python.org/downloads/) instalado na máquina. O Robot também suporta o Python 3, mas como Python 2 e 3 [não são inteiramente compatíveis](http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#python-2-vs-python-3), vamos aqui usar a versão 2.7.

Após instalar o Python, verifique se o Pip, gerenciador de pacotes do Python também está instalado. Ele deve vir instalado por padrão. Digite os comandos abaixo:


```
$ python --version
Python 2.7.12
$ pip --version
pip 9.0.1 from .local/lib/python2.7/site-packages (python 2.7)
```


Se a mensagem aparecer assim, está tudo pronto para prosseguir. Baixe o arquivo [requirements.txt](https://github.com/rodmatola/robot-framework-blog-concrete/blob/master/requirements.txt) e digite


```
$ pip install -r requirements.txt
```


Se preferir a instalação passo a passo, siga as instruções abaixo.


```
$ pip install robot-framework
```


Espere completar a instalação e verifique se tudo ocorreu bem com o comando


```
$ robot --version
Robot Framework 3.0.2 (Python 2.7.12 on linux2)
```


Usuários de Windows devem executar o Robot no cmd. Em outros terminais do Windows, como o Git Bash ou Cmder, (geralmente) não reconhecem o comando `robot`, por ser instalado como um arquivo bat.

Depois instale a biblioteca do [Selenium](http://robotframework.org/SeleniumLibrary/SeleniumLibrary.html),


```
$ pip install --pre --upgrade robotframework-seleniumlibrary
```


O upgrade é necessário para instalar a biblioteca mais nova, pois a antiga gera erros de compilação. Rode os comandos abaixo e veja se a saída contém os elementos


```
$ pip list
robotframework (3.0.2)
robotframework-ride (1.5.2.1)
robotframework-seleniumlibrary (3.0.0b3)
selenium (3.6.0)
```



### Webdrivers

Depois das nossas linguagens instaladas, precisamos baixar os drivers que controlarão os browsers utilizados. No site do [Selenium](http://www.seleniumhq.org/download/) você encontra esses drivers. Os drivers, após baixados, deverão ser descompactados e colocados na pasta `Python27/Scripts`. Funciona também se colocar na pasta do seu projeto, mas isso não é recomendado pois você teria que replicar o driver para cada projeto.

É possível, no Windows, você colocar em qualquer outra pasta, por exemplo `C:\webdrivers` (é onde eu coloco) e adicionar essa pasta ao path. Tecle “window+s”, digite “path”, clique em “Editar variáveis de sistema” > “Variáveis de Ambiente” > “Path”, e adicione o caminho onde estão seus webdrivers.

No Linux, eu coloco direto na pasta `/usr/local/bin/`.

Agora, tudo pronto para começar!


## Primeiro script

Como esse texto é uma introdução, vamos fazer uma busca simples no Google, tudo em um único arquivo. Nos próximos posts iremos adicionar mais funcionalidades e organização.

Usaremos o BDD keyword driven, e não o Gherkin, por permitir maior liberdade na escrita dos cenários. Nosso script será salvo num arquivo de extensão .robot.

Você pode baixar o fonte no meu meu [GitHub](https://github.com/rodmatola/robot-framework-blog-concrete).

Os editores que indico são o [Pycharm](https://www.jetbrains.com/pycharm/download/) (que eu uso) com os plugins Intellibot e Robot Framework Support (File>Settings>Plugins>Browser Repositories) ou o [Visual Studio Code](https://code.visualstudio.com/download) com algum plugin para o Robot, mas você pode usar outros que tenham suporte.


### *** Settings ***

Vamos começar com a seção *** Settings ***. Como vamos usar biblioteca do Selenium, é preciso declarar isso nesta seção.


```
*** Settings ***
Library  SeleniumLibrary
```


Não é obrigatório, mas é bom também colocar uma descrição da suíte, antes ou depois, não importa.


```
*** Settings ***
Documentation  Suíte de apresentação do Robot Framework
...  com uma busca simples no Google
...  para o blog da Concrete

Library  SeleniumLibrary
```



### *** Test Cases ***

Aqui é onde nossos cenários e casos de teste, em linguagem natural, serão descritos. 

Esta parte não tem muito segredo, só escreva o nome do cenário como se fosse um título, alinhado à esquerda, e os passos indentados abaixo.


```
*** Test Cases ***
Cenário: Buscar por Robot Framework no Google e entrar na página
   [Documentation]  Cenário para uma busca sobre Robot Framework
   ...              no Google
   [Tags]  busca
   Abrir o Chrome na página <http://www.google.com.br>
   Digite "Robot Framework" no campo de busca
   Verifique se a busca retornou o resultado esperado
   Clique no primeiro link
   Verifique se a página mostrada e a correta
   Fechar o browser
```


Note que, diferente da seção *** Settings ***, aqui a palavra-chave [Documentation] está entre colchetes. Note também que as reticências de continuação de linha estão indentadas.

Agora, vamos a implementação do cenário.


### *** Keywords ***

Nesta seção é onde você implementa todas as suas palavras-chave (ou frases-chave). A implementação segue o mesmo formato acima, com a palavra-chave alinhada a esquerda e a implementação indentada abaixo. Vou colocar toda a implementação e depois explicarei linha por linha.


```
*** Keywords ***
Abrir o ${browser} na página <${url}>
   Open Browser  ${url}  ${browser}

Digite "${palavra busca}" no campo de busca
   Set Test Variable  ${palavra busca}
   Input text      id = lst-ib  ${palavra busca}
   Click Button    Pesquisa Google

Verifique se a busca retornou o resultado esperado
   ${titulo} =  Get title
   Should Contain  ${titulo}  ${palavra busca}

Clique no primeiro link
   ${texto} =  Get text  css = #rso > div:nth-child(1) > div > div > div > div > h3
   Click link  ${texto}

Verifique se a página mostrada e a correta
   ${titulo}                   Get title
   Should Contain              ${titulo}       ${palavra busca}
   Element text should be      css = h1        ROBOT FRAMEWORK

Fechar o browser
   Close browser

```


Compare o que está dentro dos *** Test Cases *** com o que está nas *** Keywords ***.

***** Test Cases *****

Abrir o Chrome na página &lt;[http://www.google.com.br](http://www.google.com.br)>

Digite "Robot Framework" no campo de busca

***** Keywords *****

**Abrir o ${browser} na página &lt;${url}>**

**Digite "${palavra busca}" no campo de busca**

Veja que o Robot reconhece os parâmetros a serem passados só pela posição na frase. Basta você substituir o parâmetro pelo nome que você quer dar a variável. Não precisa de nenhum comando ou caracter especial para isso.

Cada variável fica no contexto da sua palavra-chave. `${browser}` e `${url}` só existem dentro de **Abrir o ${browser} na página &lt;${url}>**. Para a variável ser visível em outros contextos, é preciso especificar isso. No nosso caso, usei o `Set Test Variable`, para torná-la visível dentro do nosso cenário. Existem também o `Set Suite Variable` e o `Set Global Variable`, mas não vamos falar deles aqui.

O Robot é “esperto” o suficiente para saber onde você quer clicar, como no Click Button    _Pesquisa Google_. Não é preciso informar o css selector ou o xpath. O mesmo vale para o Click Link mais abaixo.

A verificação do resultado da busca é feita pelo título da página, que aparece escrito na aba do browser. Ao clicar no primeiro link da busca, a verificação é feita pelo título e pelo tag H1, já que esta tag deve ser única.

A suíte termina com o fechamento do browser.


## Reports e logs

Após a execução dos testes, o Robot gera um Report e um Log em html, bem completo. Se algum teste falhar, olhe o que aconteceu no log. Certamente ele ajudará. 

É possível fazer algumas customizações nesses arquivos, mas não vou entrar em detalhes aqui. Aconselho que os explorem, principalmente o log.

O Robot se mostra como mais uma opção de frameworks para automação de testes. Não se prende a uma sintaxe exclusiva, como o Cucumber/Gherkin, sendo uma alternativa ao Ruby.


## Para saber mais



*   Site do [Robot Framework](http://robotframework.org/)
*   [Documentação da Selenium Library](http://robotframework.org/SeleniumLibrary/SeleniumLibrary.html)
*   Blog da Mayara, [Robotizando testes](http://robotizandotestes.blogspot.com.br/)
