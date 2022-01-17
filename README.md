<!--
Projeto de Recurso de Linguagens de Programação II 2021/2022 (c) by Nuno Fachada

Projeto de Recurso de Linguagens de Programação II 2021/2022 is licensed under a
Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.

You should have received a copy of the license along with this
work. If not, see <http://creativecommons.org/licenses/by-nc-sa/4.0/>.
-->

# Projeto de Recurso de Linguagens de Programação II 2021/2022

## Descrição do problema

Os grupos devem implementar o jogo [Madelinette](#madelinette) para dois
jogadores na forma de um projeto de consola em C# (.NET 5.0) e de um projeto
Unity (2021.1).

A lógica, regras e dados do jogo (o chamado **modelo**), devem ser completamente
independentes tanto da interface de consola (`WriteLines`, `ReadLines`), como
do Unity. Este modelo deve ser obrigatoriamente o mesmo em ambas as
implementações, devendo ser partilhado de uma forma que não implique
copiar os ficheiros de um lado para o outro. Existe forma de fazer isto ao
nível do Git usando [sub-módulos][submodules], como foi feito
[neste caso][ia-simplexity], por exemplo.

O [Model-View-Controller (MVC)][MVC] *design pattern* é necessário para a
realização deste projeto. Por exemplo, [neste projeto Unity][ia-simplexity], o
modelo é completamente independente do Unity, sendo trivial implementar uma
versão em consola.

O exemplo [MVCExample] e o [respetivo vídeo de desenvolvimento][MVCVideo] são
essenciais para a compreensão do que é necessário fazer neste projeto. No
entanto tenham atenção que a implementação em consola desse exemplo é muito
simples e não faz uso dos *design patterns*
[necessários para a implementação em consola](#detalhes-adicionais-do-projeto-em-consola)
deste projeto.

O objetivo principal deste projeto é a implementação de uma aplicação com uma
arquitetura bem pensada e bem desenhada. Obviamente que o jogo deve funcionar
corretamente, mas isso por si só não serve de nada se os
[requisitos técnicos obrigatórios](#requisitos-técnicos-obrigatórios),
nomeadamente a nível de arquitetura, não tiverem sido cumpridos.

## Madelinette

O Madelinette é um jogo de tabuleiro para dois jogadores muito simples. Os
jogadores podem mover as suas peças que estejam adjacentes à única posição livre
do tabuleiro. O objetivo é bloquear o outro jogador de conseguir fazer jogadas.
Quando um jogador fica bloqueado, perde, sendo que o último jogador a
conseguir jogar, vence.

![Madelinette](madelinette.png)

Alguns _links_ sobre o jogo:

* [Madelinette no BoardGameGeek](https://boardgamegeek.com/boardgame/25848/madelinette)
* [Versão jogável online](http://www.lutanho.net/play/madelinette.html)

## Requisitos da aplicação

### Requisitos técnicos obrigatórios

Os seguintes requisitos são obrigatórios. Caso não sejam tidos em conta, a nota
do projeto é zero.

#### Arquitetura de classes e _patterns_

O projeto deve obedecer aos princípios [SOLID], bem como aos princípios gerais
de design de classes. Devem usar o [MVC] *pattern* para organizar o código, e o
código do **modelo** (o **M** em MVC) deve ser o mesmo entre as duas
implementações (consola e Unity).

#### Funcionamento da aplicação

A nível de jogabilidade/interação:

* O jogo tem de funcionar corretamente.
* A aplicação deve ter um menu inicial com as opções "Play", "Rules", "Authors"
  e "Quit".
* A UI do jogo deve ser intuitiva, colocando em cima das peças jogáveis um
  número que, se pressionado, move a respetiva peça para a posição livre. No
  Unity podem também usar o clique do rato para mover a peça.
* Durante o jogo, devem existir opções para voltar ao menu principal e para
  sair imediatamente da aplicação, e as mesmas devem estar claramente indicadas
  no UI.

#### Detalhes adicionais do projeto em consola

O projeto de consola tem de fazer uso e implementar corretamente os seguintes
_design patterns_, além do MVC:

* Double buffer
* Game loop
* Update method
* Component pattern

Além disso:

* Tal como o jogo em si, o menu deve ser uma cena (_scene_) a correr no
  _game loop_, tal como seria se a aplicação fosse implementada em Unity.
* Não podem usar `Console.ReadLine()` ou `Console.Read()`, nem seria possível
  fazê-lo com o _game loop pattern_. Podem usar apenas `Console.ReadKey(true)`
  numa _thread_ separada, tal como discutido nas aulas.
* Tanto o menu como o jogo devem ter algum tipo de animação de modo a que se
  perceba que estamos num _game loop_ e não numa aplicação passiva de consola
  (daquelas que funcionam com `Console.ReadLine()`).
* O projeto deve ser multi-plataforma, ou seja, deve funcionar em Linux e macOS,
  pelo que devem ser evitados métodos e classes que apenas funcionam em Windows.

A aplicação de consola deve compilar sem _warnings_. Podem compilar o projeto
sem o executar com `dotnet build nome-do-projeto` na pasta da solução (ou apenas
`dotnet build` na pasta do projeto).

#### Detalhes adicionais do projeto de Unity

A aplicação Unity deve compilar e executar sem erros e _warnings_.

### Requisito técnico para nota superior a 8 (num máximo de 10)

O tabuleiro do jogo não é um _array_ bidimensional perfeito, pois não existe
ligação horizontal nas duas peças de cima. Desta forma, implementar o tabuleiro
usando um _array_ bidimensional é uma forma pouco ótima de o fazer, pois requer
um `if` para verificar essa exceção em particular, além de que requer muitos
`if`s para verificar cada uma das posições e situações possíveis. Se quisessem
alterar a estrutura do tabuleiro para experimentar outro tipo de jogo, teriam de
mudar o código todo.

A alternativa é criarem uma estrutura de dados chamada grafo, na qual cada nó
(posição) tem vizinhos pré-especificados (outros nós). Além dos seus vizinhos,
cada nó (posição) pode estar numa de três situações: com peça branca, com peça
preta, ou sem peça. Desta forma, para saberem se um nó (posição), tem uma peça
jogável, basta verificarem que peça lá está e percorrer os seus nós (posições)
vizinhos no grafo, verificando se algum deles é a posição vazia. Nada impede que
o vosso grafo não funcione também diretamente com coordenadas, caso implementem
indexadores para o efeito.

Caso não implementem uma estrutura de dados deste tipo, a nota máxima do projeto
fica limitada a 8 valores.

### Sugestões para uso de sub-módulos em Git

Para fazerem _clone_ de um projeto com sub-módulos devem usar o seguinte
comando, exemplificado para o projeto exemplo [MVCExample], cujo vídeo de
desenvolvimento podem visualizar [neste link][MVCVideo]:

```
git clone --recurse-submodules https://github.com/VideojogosLusofona/MVCExample.git
```

Caso se tenham esquecido de usar a opção `--recurse-submodules`, podem executar
o seguinte comando na raiz do projeto que obtém os conteúdos dos sub-módulos:

```
git submodule update --init --recursive
```

Os sub-módulos estão inicialmente no estado `HEAD detached`, isto é, não estão
em nenhum ramo. Para os sub-módulos ficarem no ramo pretendido, por exemplo o
ramo `common`, basta fazer `cd` até à pasta de cada sub-módulo e fazer
`git checkout common` (e depois `git pull` para obter as últimas alterações
ou `git add/commit/push` para criarem _commits_ específicos ao sub-módulo).

<!-- git submodule foreach --recursive git checkout common-->

## Objetivos e critério de avaliação

Este projeto tem os seguintes objetivos:

* **O1** - O projeto deve funcionar tal como indicado na secção
  [Funcionamento da aplicação](#funcionamento-da-aplicação).
* **O2** - Projeto deve ser implementado segundo o que está definido na secção
  [Arquitetura de classes e _patterns_](#arquitetura-de-classes-e-patterns), e
  se possível o
  [requisito técnico para nota superior a 8](#requisito-técnico-para-nota-superior-a-8-num-máximo-de-10).
  Além disso, o projeto deve ter em conta o seguinte:
  * Código devidamente comentado e indentado.
  * Inexistência de código "morto", que não faz nada, como por exemplo
    variáveis, propriedades ou métodos nunca usados.
* **O3** - Código devidamente indentado, comentado e documentado. Documentação
  deve ser feita com [comentários de documentação XML][XML].
* **O4** - Repositório Git deve refletir boa utilização do mesmo, com
  *commits* de todos os elementos do grupo e mensagens de *commit* que sigam
  as melhores práticas para o efeito (como indicado
  [aqui](https://chris.beams.io/posts/git-commit/),
  [aqui](https://gist.github.com/robertpainsi/b632364184e70900af4ab688decf6f53),
  [aqui](https://github.com/erlang/otp/wiki/writing-good-commit-messages) e
  [aqui](https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)).
  Quaisquer *assets* binários, tais como imagens, devem ser integrados
  no repositório em modo Git LFS.
* **O5** - Relatório em formato [Markdown] (ficheiro `README.md`),
  organizado da seguinte forma:
  * Título do projeto.
  * Autoria:
    * Nome dos autores (primeiro e último) e respetivos números de aluno.
    * Informação de quem fez o quê no projeto. Esta informação é
      **obrigatória** e deve refletir os *commits* feitos no Git.
  * Arquitetura da solução:
    * Descrição da solução, com breve explicação de como o programa foi
      organizado, indicação dos *design patterns* utilizados e a razão do
      seu uso, bem como dos algoritmos implementados (e.g., para procura de
      peças jogáveis, etc).
    * Um diagrama UML de classes simples (i.e., sem indicação dos
      membros da classe) descrevendo a estrutura de classes.
  * Referências, incluindo trocas de ideias com colegas, código aberto
    reutilizado (e.g., do StackOverflow) e bibliotecas de terceiros
    utilizadas. Devem ser o mais detalhados possível.
  * **Nota:** o relatório deve ser simples e breve, com informação mínima e
    suficiente para que seja possível ter uma boa ideia do que foi feito.
    Atenção aos erros ortográficos e à correta formatação [Markdown], pois
    ambos serão tidos em conta na nota final.

O projeto tem um peso de 10 valores na nota final da disciplina e será avaliado
de forma qualitativa. Isto significa que todos os objetivos têm de ser
parcialmente ou totalmente cumpridos. A cada objetivo, O1 a O5, será atribuída
uma nota entre 0 e 1. A nota do projeto será dada pela seguinte fórmula:

*N = 10 x O1 x O2 x O3 x O4 x O5 x D*

Em que *D* corresponde à nota da discussão e percentagem equitativa de
realização do projeto, também entre 0 e 1. Isto significa que se os alunos
ignorarem completamente um dos objetivos, não tenham feito nada no projeto ou
não comparecerem na discussão, a nota final será zero.

## Entrega

O projeto deve ser submetido no Moodle até às **23h00 de 6 de fevereiro de
2022**. Certifiquem-se de que a aplicação está funcional e que todos
os requisitos foram cumpridos, caso contrário o projeto não será avaliado.

O repositório deve ter:

* Projeto de consola .NET 5.0 funcional.
* Projeto de Unity 2021.1.x funcional.
* Ficheiros `.gitignore` e `.gitattributes` adequados para o projeto ou projetos
  em questão.
* Ficheiro `README.md` contendo o relatório do projeto em formato [Markdown].
* Ficheiros de imagens, contendo o diagrama UML de classes e outras figuras
  que considerem úteis. Estes ficheiros devem ser incluídos no repositório em
  modo Git LFS.

Em nenhuma circunstância o repositório pode ter _builds_ ou outros ficheiros
temporários dos projetos, do Visual Studio e do Unity, que são automaticamente
ignorados se usarem um `.gitignore` apropriado.

## Honestidade académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de
honestidade académica. Isto significa que cada ideia que não seja do
aluno deve ser claramente indicada, com devida referência ao respetivo
autor. O não cumprimento desta regra constitui plágio.

O plágio inclui a utilização de ideias, código ou conjuntos de soluções
de outros alunos ou indivíduos, ou quaisquer outras fontes para além
dos textos de apoio à disciplina, sem dar o respetivo crédito a essas
fontes. Os alunos são encorajados a discutir os problemas com outros
alunos e devem mencionar essa discussão quando submetem os projetos.
Essa menção **não** influenciará a nota. Os alunos não deverão, no
entanto, copiar códigos, documentação e relatórios de outros alunos, ou dar os
seus próprios códigos, documentação e relatórios a outros em qualquer
circunstância. De facto, não devem sequer deixar códigos, documentação e
relatórios em computadores de uso partilhado.

Nesta disciplina, a desonestidade académica é considerada fraude, com
todas as consequências legais que daí advêm. Qualquer fraude terá como
consequência imediata a anulação dos projetos de todos os alunos envolvidos
(incluindo os que possibilitaram a ocorrência). Qualquer suspeita de
desonestidade académica será relatada aos órgãos superiores da escola
para possível instauração de um processo disciplinar. Este poderá
resultar em reprovação à disciplina, reprovação de ano ou mesmo suspensão
temporária ou definitiva da ULHT.

*Texto adaptado da disciplina de [Algoritmos e
Estruturas de Dados][aed] do [Instituto Superior Técnico][ist]*

## Referências

* \[1\] Whitaker, R. B. (2021). **The C# Player's Guide** (4th Edition).
  Starbound Software.
* \[2\] Albahari, J., & Johannsen, E. (2020). **C# 8.0 in a Nutshell**. O’Reilly
  Media.
* \[3\] Freeman, E., Robson, E., Bates, B., & Sierra, K. (2020). **Head First
  Design Patterns** (2nd edition). O'Reilly Media.
* \[4\] Dorsey, T. (2017). **Doing Visual Studio and .NET Code Documentation
  Right**. Visual Studio Magazine. Retrieved from
  <https://visualstudiomagazine.com/articles/2017/02/21/vs-dotnet-code-documentation-tools-roundup.aspx>.

## Licenças

Este enunciado é disponibilizado através da licença [CC BY-NC-SA 4.0].

## Metadados

* Autor: [Nuno Fachada]
* Curso:  [Licenciatura em Videojogos][lamv]
* Instituição: [Universidade Lusófona de Humanidades e Tecnologias][ULHT]

[CC BY-NC-SA 4.0]:https://creativecommons.org/licenses/by-nc-sa/4.0/
[lamv]:https://www.ulusofona.pt/licenciatura/videojogos
[Nuno Fachada]:https://github.com/fakenmc
[ULHT]:https://www.ulusofona.pt/
[aed]:https://fenix.tecnico.ulisboa.pt/disciplinas/AED-2/2009-2010/2-semestre/honestidade-academica
[ist]:https://tecnico.ulisboa.pt/pt/
[Markdown]:https://guides.github.com/features/mastering-markdown/
[Doxygen]:https://www.stack.nl/~dimitri/doxygen/
[DocFX]:https://dotnet.github.io/docfx/
[KISS]:https://en.wikipedia.org/wiki/KISS_principle
[XML]:https://docs.microsoft.com/dotnet/csharp/codedoc
[SOLID]:https://en.wikipedia.org/wiki/SOLID
[Game Loop]:http://gameprogrammingpatterns.com/game-loop.html
[Update Method]:http://gameprogrammingpatterns.com/update-method.html
[Component Pattern]:https://gameprogrammingpatterns.com/component.html
[Roslynator]:https://marketplace.visualstudio.com/items?itemName=josefpihrt-vscode.roslynator
[MVC]:https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
[ia-simplexity]:https://github.com/VideojogosLusofona/color-shape-links-ai-competition
[submodules]:https://git-scm.com/book/en/v2/Git-Tools-Submodules
[MVCExample]:https://github.com/VideojogosLusofona/MVCExample.git
[MVCVideo]:https://www.youtube.com/watch?v=_z_iRUjmvzE
