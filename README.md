**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

*Linguagens de Programação I - 2019/2020*

# MiniProjecto - CifreX

Na resolução desta ficha deve ser utilizada a Linguagem de Programação C. Para além da correta implementação dos requisitos, tenha em conta os seguintes aspetos:

- O código apresentado deve ser bem indentado. 
- O código deve compilar sem erros ou *warnings* utilizando o *gcc* com as seguintes flags:
 `-Wall -Wextra -Wpedantic -std=c99`
- Tenha em atenção os nomes dados das variáveis, para que sejam indicadores daquilo que as mesmas vão conter.
- Evite o uso de constantes mágicas. 
- Evite duplicação de código. 
- Considere a implementação de funções para melhorar a legibilidade, evitar a duplicação e criar soluções mais genéricas.
- É proíbida a utilização de variáveis globais - i.e. variáveis declaradas fora de qualquer função.
- Este trabalho poderá ser realizado individualmente ou em grupos com o máximo de 2 alunos.

## 1. Introdução

Neste trabalho pretence-se que os alunos trabalhem com alguns algortimos de encriptação muito simples.

Encriptação é o processo de transformar informação usando um algoritmo (chamado cifra) de modo a impossibilitar a sua leitura a todos excepto aqueles que conheçam a forma de desencriptar, geralmente referida como chave. O resultado deste processo é uma informação **encriptada**, também chamada de texto cifrado [(4)](#ref4).

As mensagens a serem encriptadas terão o **máximo** de 166 caracteres, o número de caracteres de um *tweet*. Lembre-se que as strings em C têm sempre um caracter de terminação invisível, ou seja `\0`.

## Funcionamento do programa

Pretende desenvolver-se um programa para que aplica dois tipos de cifras a um texto lido do teclado. O programa deverá ler do stdin um input com o seguinte formato:
`<op> <n> <texto>`
onde

* `<op>`: é um caracter que especifica o comportamento do programa:
   - `s` : o programa deverá aplicar ao `<texto>` a cifra de substituíção (ver Secção [2.1](#subst)) e apresentar o resultado na consola.
   - `S` : o programa deverá desencriptar o `texto` assumindo que este está encriptado com a cifra substituíção. Deverá apresentar o resultado na consola.
   - `t` : o programa deverá aplicar ao `texto` a cifra de transposição (ver Secção [2.2](#transp)) e apresentar o resultado na consola. 
   - `T` : o programa deverá desencriptar o `texto` assumindo que este está encriptado com a cifra transposição. Deverá apresentar o resultado na consola.
   - `e` : o programa deverá aplicar ao `texto` a cifra de transposição II (ver Secção [2.3](#transp2)) e apresentar o resultado na consola. Esta opção é uma opção extra e a avaliação desta funcionalidade será feita separadamente - (ver Secção [2.3](#transp2))
   - `E` : o programa deverá desencriptar o `texto` assumindo que este se encontra encriptado com a cifra de transposição II (ver Secção [2.3](#transp2)) e apresentar o resultado na consola. Esta opção é uma opção extra e a avaliação desta funcionalidade será feita separadamente - (ver Secção [2.3](#transp2))
   - `q` : o programa deverá terminar, apresentado a mensagem: `Exiting->`
   - Caso o utilizador introduza outro caracter (sem ser um dos caracteres esperados) o programa deverá imprimir a seguinte mensagem de erro: `Error: Unkown option` e deverá voltar a esperar um novo *input* do utilizador. O programa deverá ignorar espaços em branco introduzidos antes do caracter `<op>`.
* `<n>`: No caso da cifra de deslocamento, este parâmetro indica o número de caracteres de deslocamento a aplicar. No caso da cifra de transposição, indica o número de colunas da matriz. 
* `<texto>`: é uma string, com o máximo de 166 caracteres, que pode conter espaços.

Depois de aplicar uma cifra o programa deverá apresentar o resultado no terminal e voltar a ficar à espera que o utilizador faça um novo *input*. O programa só deverá terminar quando o utilizador introduz o caracter `q`. 

### 2.1 Cifra de substituição<a name="subst"></a>

Uma cifra de substituição é um algoritmo que permite a encriptação substituindo um dado caracter por outro. A forma como se encontra a correspondência de um caracter do alfabeto para outro pode ser definido através de um *shift* do alfabeto para a direita. Por exemplo uma cifra de substituição *5* tem a seguinte correspondência:

`0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ` - Alfabeto  
`56789ABCDEFGHIJKLMNOPQRSTUVWXYZ01234` - Alfebeto Cifrado

`CADA MALLOC PRECISA DE UM FREE` - Frase original  
`HFIF RFQQTH UWJHNXF IJ ZR KWJJ` - Frase encriptada

Nesta versão simplificada apenas os caracteres `0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ` são encriptados. Desta forma, o texto deverá ser convertido para letras maiúsculas e todos os caracteres que não pertencem ao grupo dos caracteres encriptados deverão manter-se inalterados - como é o caso dos espaços no exemplo anterior.
Caso o utilizador especifique um número positivo, o deslocamento é ser feito para a direita (como no caso do exemplo). Caso o número seja negativo, o deslocamento é feito para a esquerda.

O utilizador poderá introduzir o texto utilizando letras maiúsculas ou minúsculas, mas o resultado deverá ser sempre apresentado em maiúsculas.

O parâmetro `n` introduzido pelo utilizador terá obrigatoriamente que estar no intervalo [-35 ; 35]. Caso o número esteja fora do intervalo o programa deverá imprimir a seguinte mensagem de erro: `Error: out of bound` e deverá voltar a esperar um novo *input* do utilizador. 

### 2.2 Cifra de Transposição<a name="transp"></a>

Ao contrario da cifra de substituição, numa cifra de transposição as letras existentes na mensagem continuam a ser as mesmas que na mensagem original. Contudo estas letras são mostradas numa ordem diferente.

Uma cifra de transposição simples pode ser feita com recurso a uma matriz. O texto é disposto sobre a forma matricial. Essa matriz é transposta e o texto cifrado é extraído dessa matriz.

Exemplo:

Texto original: 

```
"Sou um guardador de rebanhos."
```

Texto disposto em forma matricial numa matriz com 4 colunas:

```
+---+---+---+---+
| S | o | u |   |
+---+---+---+---+
| u | m |   | g |
+---+---+---+---+
| u | a | r | d |
+---+---+---+---+
| a | d | o | r |
+---+---+---+---+
|   | d | e |   |
+---+---+---+---+
| r | e | b | a |
+---+---+---+---+
| n | h | o | s |
+---+---+---+---+
| . |   |   |   |
+---+---+---+---+
```

Matriz transposta:
```
+---+---+---+---+---+---+---+---+
| S | u | u | a |   | r | n | . |
+---+---+---+---+---+---+---+---+
| o | m | a | d | d | e | h |   |
+---+---+---+---+---+---+---+---+
| u |   | r | o | e | b | o |   |
+---+---+---+---+---+---+---+---+
|   | g | d | r |   | a | s |   |
+---+---+---+---+---+---+---+---+
```

Frase Cifrada:
```"Suua rn.omaddeh u roebo  gdr as "```

O parâmetro `n` introduzido pelo utilizador terá obrigatoriamente que estar no intervalo [2 ; 50]. Caso o número esteja fora do intervalo o programa deverá imprimir a seguinte mensagem de erro: `Error: out of bound` e deverá voltar a esperar um novo *input* do utilizador. 

### 2.3 Extra - Cifra de Transposição<a name="transp2"></a>

Um outro dipo de  cifra de transposição simples pode ser descrita da seguinte forma:

Considere uma string ***S***, constituida por *N* carateres, identificados na forma ***S*** = *S<sub>1</sub>S<sub>2</sub>...S<sub>N</sub>*

1. Se o comprimento de ***S*** é 1 ou 2 então a função **encript**(***S***) = ***S***, isto é, a própria string.
2. Se ***S*** for uma string de comprimento *N* > 2 então **encript**(***S***) = **encript**(S<sub>k</sub>...S<sub>2</sub>S<sub>1</sub>) + **encript**(*S<sub>N</sub>S<sub>N-1</sub>...S<sub>k+1</sub>*) **+**  em que:
    - *k* = *N*/2, divisão inteira
    - símbolo **+** significa concatenação de strings, e.g. `AB+CB=ABCD`

Exemplos:

```C
encript("OK") = "OK"
```

```C
encript("12345678") = encript("4321") + encript("8765") 
                    = encript("34") + encript("12") + encript("78") + encript("56") 
                    = "34127856"
```

Neste caso, todos os caracteres do `texto` deverão ser encriptados e o parâmetro `n` não deverá ser lido.

Esta cifra implementa-se naturalmente usando uma função recursiva. Uma declaração possível para esta função é a seguinte:
```C
char * encrypt(char * s, const int length);
```
Será também conveniente a implementação de uma função que inverta uma string com a seguinte declaração:

```C
char * reverse(char * s, const unsigned int n);
```

A implementação desta funcionalidade é opcional e será avaliada, num *constest* separado no PANDORA [(2)](#ref2). Os alunos que optarem por fazer esta implementação poderão substituir a nota de um dos exercícios práticos pela nota obtida nesta implementação.

Os alunos não terão melhor nota no Mini Projecto pela implementação desta funcionalidade. Apenas poderão substituir a nota obtida num dos exercícios práticos.

## Exemplo de utilização do programa

O exemplo seguinte demonstra o funcionamento do programa
```shell_session
$>./cifreX
s 9 programming is the art of copy and paste.
Y0XP0JVVRWP R1 2QN J02 XO LXY7 JWM YJ12N.
S 9 Y0XP0JVVRWP R1 2QN J02 XO LXY7 JWM YJ12N.
PROGRAMMING IS THE ART OF COPY AND PASTE.
t 7 Simplicity is prerequisite for reliability.
Sipioa.itrsrb myei i p rtrl lieeei isq lt c ufiy 
T 7 Sipioa.itrsrb myei i p rtrl lieeei isq lt c ufiy
Simplicity is prerequisite for reliability.      
e If debugging is the process of removing software bugs, then programming must be the process of putting them in.    
nvisg  ofmoreu b, gstofrewaigg ing Ifbudecros ests  pheos puf r pesocmhen. iitt tngrogmiamethprn e bhe t ngstmu
E nvisg  ofmoreu b, gstofrewaigg ing Ifbudecros ests  pheos puf r pesocmhen. iitt tngrogmiamethprn e bhe t ngstmu
If debugging is the process of removing software bugs, then programming must be the process of putting them in.
q
Exiting->
```

## Material a entregar

* Ficheiro `.c` com código devidamente comentado e indentado:
    - Deve implementar as funcionalidades pedidas.
    - O código deverá ser submetido na plataforma PANDORA [(2)](#ref2) até às **23:59 do dia 10 de Maio de 2020** no *contest* **LP12020MP**.
    - O código que implementa a funcionalidade **extra** deverá ser submetido na plataforma PANDORA [(2)](#ref2) até às **23:59 do dia 17  de Maio de 2020** no *contest* **LP12020MPExtra**.
         - Apenas para os alunos que queiram substituir a nota de um dos exercícios práticos ou que queiram fazer por gosto pessoal.
         - Podem fazer esta submissão individualmente ou em grupo de 2 (pode ser um grupo diferente do grupo do MiniProjecto).
         - A submissão no contest **LP12020MPExtra** não substitui a entrega no **LP12020MP**.
- A plataforma corre automaticamente uma série de testes e no fim atribui uma classificação **indicativa**. Os alunos deverão analisar o relatório emitido pela plataforma e poderão alterar o código e voltar a submeter o trabalho. Neste trabalho não haverá limite de submissões.
      A plataforma não permite a entrega de trabalhos após a data e hora limite.
* Ficheiro `relatorio_<nome_do_grupo>.pdf` em formato [PDF].
    - O relatório deverá ser submetido no moodle [(3)](#ref3) até às  **23:59 do dia 11 de Maio de 2020**. 
    - O nome do ficheiro *tem que respeitar o formato*. Por exemplo, se o nome do grupo for `king_mackerel`, o ficheiro deverá chamar-se `relatorio_king_mackerel.pdf`.
    - Incorrecta indentação do código poderá originar penalizações na nota.


## Relatório e Peer Review

Cada grupo deverá produzir e entregar um relatório cujo objectivo é explicar em detalhe a solução encontrada. Note que um relatório não deve conter código. Se for absolutamente essencial, pode, pontualmente, mostrar trechos de código.

O relatório deve conter *no mínimo* as seguintes secções:
   * Título
   * Descrição da solução
       * Fluxograma das funções principais
   * Conclusões e matéria aprendida
   * Referências
      * Incluindo trocas de ideias com colegas, código aberto reutilizado e bibliotecas utilizadas

Neste trabalho iremos aplicar uma técnica de *peer review* dos relatórios. Consequentemente os relatórios terão de ser anónimos - **os relatórios não podem conter os nomes, nem os números dos autores**. Apenas o nome do ficheiro terá de respeitar o formato: `relatorio_<nome_do_grupo>.pdf`. Ficheiros cujo nome não respeite o formato pedido ou que não estejam em formato PDF não serão aceites.

Cada aluno (individualmente) terá que rever e avaliar dois relatórios de colegas seus. Esta avaliação consiste no preenchimento de um ficheiro excel com critérios estipulados. Este ficheiro será disponibilizado na plataforma Moodle.

Após a submissão dos relatórios no Moodle, procederemos a uma distribuição dos relatórios pelos alunos. Os alunos deverão fazer a sua revisão e avaliação dos dois relatórios e submeter no Moodle o ficheiro excel preenchido.

Alunos que não submetam a sua avaliação dos relatórios do colegas, não terão nota no Mini Projecto.

Como diria Peter Parker, *With great power comes great responsability*. Acreditamos que este processo será benéfico na formação dos alunos e no desenvolvimento do seu pensamento crítico. No entanto, qualquer suspeita de fraude no processo de avaliação dos relatórios será investigada e, em caso de confirmação, os alunos envolvidos não terão avaliação neste projecto. 

## Peso na avaliação

O projecto vale 15% da nota final e será cotado de 0 a 20 valores. A avaliação do mesmo será feita da seguinte forma:

* 17  valores - Código (funcionalidade, indentação, comentários) - A classificação do Pandora servirá apenas de referência.
* 3 valores - Relatório.

## Honestidade Académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de honestidade académica. Trabalhos que sejam identificados como cópias serão anulados e os alunos envolvidos terão nota zero - quer tenham copiado, quer tenham deixado copiar.
Para evitar situações deste género, recomendamos aos alunos que nunca partilhem ou mostrem o seu código.
A decisão sobre se um trabalho é uma cópia cabe exclusivamente aos docentes da unidade curricular.
Os alunos são encorajados a discutir os problemas com outros alunos mas não deverão, no entanto, copiar códigos, documentação e relatórios de outros alunos. Em nenhuma circunstância deverão partilhar os seus próprios códigos, documentação e relatórios. De facto, não devem sequer deixar códigos, documentação e relatórios em computadores de uso partilhado.

## Referências

<a name="ref1"></a>

* (1) Pereira, A. (2017). C e Algoritmos, 2ª edição. Sílabo.

<a name="ref2"></a>

* (2)  PANDORA - Yet Another Automated Assessment Tool, https://saturn.ulusofona.pt/.

<a name="ref3"></a>

* (3)  Lusófona Moodle, https://secure.grupolusofona.pt/ulht/moodle/

<a name="ref3"></a>
* (3)  Conceito de Encriptação - [https://pt.wikipedia.org/wiki/Encripta%C3%A7%C3%A3o](https://pt.wikipedia.org/wiki/Encriptação)

## Metadados

* Autor: [Guilherme Sanches, Pedro Serra, Hugo Castro, Lucio Studer]
* Curso:  [Licenciatura em Engenharia Informática][lei]
* Curso:  [Licenciatura em Engenharia Informática e Redes de Telecomunicações][leirt]
* Curso:  [Licenciatura em Informática de Gestão][lig]
* Instituição: [Universidade Lusófona de Humanidades e Tecnologias][ULHT]
