# clean-code-anotacoes
Anotações feitas durante e após leitura do livro Clean code.


# 1- Introdução

O que é código limpo ? 

Código sem duplicação, código que é possível entender as abstrações e que esclareça as dúvidas a medida que é lido.

É um código que deve se auto explicar.

Quando o código é limpo, é claro de se perceber os princípios do SOLID dentro da estrutura do código. Responsabilidade única, Aberto a extensão e fechado a modificação, inversão de dependência, injeção de dependência, etc.

Regra do escoteiro: Deixar o código que pegamos, mais limpo do que estava.

# 2- Nomes significativos

Escreva nomes de métodos baseado no domínio da implementação.

Expresse a linguagem de negócio através dos conceitos utilizados para a implementação.

Assinatura → PADES e CADES →ManipuladorAssinaturaPades e ManipuladorAssinaturaCades;

# 3- Funções (métodos)

Não se apegar ao tamanho ou complexidade, mas sim se o método faz o que ele deveria fazer, utilizar o princípio de segregação de interfaces.

Um método deve fazer apenas uma coisa, se ele fizer mais de uma coisa E SE essas duas coisas forem interligadas, deve-se criar uma abstração que faça sentido englobar os dois conceitos em um só, caso contrário, separar a responsabilidade.

Assinar → Deve apenas assinar. → Dentro do conceito de assinar existe a criação de hash, assinar o hash e injetar o hash.

Fluxo de assinar gera o hash, assina o hash e injeta o hash de maneira separada, no caso específico algumas operações necessitam ser assim devido ao contexto da aplicação. Juntamos fluxos quando conseguimos para melhorar performance, se não, mantemos dessa maneira.

Parâmetros de funções → Reduzir quando possível, utilizar DTOs para carregar a informação caso seja de um fluxo onde faça sentido. Evitar muitos parâmetros dentro de um método, cada parâmetro aumenta demasiadamente a complexidade do método, levando a um alto tempo de entendimento para o desenvolvedor e acoplando assinaturas de métodos.

# 4- Comentários

Por padrão já acho ruim adicionar comentários, caso seja necessário, é preferível uma documentação a parte como já utilizamos na Attus do que comentários dentro do código.

A necessidade dos comentários já aciona um code smell, então somente em ultimos casos.

Na attus não costumamos adicionar comentários, entendo que seja de cultura de empresa para com o projeto.

Caso entre na linha e adicione um comentário, fazer questão que o comentário não será defasado num futuro próximo e não repetir coisas que o código já pode explicar.

# 5- Formatação

Formatação vertical → Métodos de conceitos próximos devem ficar próximos um do outro para facilitar leitura.

O código se lê de cima para baixo, então manter métodos publicos em cima, com maior nível de abstração, e métodos privados em baixo, mais baixo nível de implementação.

Facilita a leitura do código para a próxima pessoa que desenvolverá.

# 6- Estrutura de dados

Depender da abstração ao invés de expor implementação.

Objeto x estrutura de dados

Estrutura de dados devem expor seus dados e esconder como operam

Objetos devem esconder seus dados e expor como operam;

DTOs → Objeto de transferencia de dados

# 7- Tratamento de erro

try-catch-finally

Estruturar aninhadamente exceções e utilizar uma classe wrapper para agrupar os códigos e não sobrescrever todas as exceções em uma mensagem genérica.

Uma mensagem de erro deve especificar ao usuário e ao desenvolvedor onde o erro acontece.

Evitar trabalhar com parâmetros nulos, seja enviando nulo ou retornando nulo de uma função, em uma linguagem tipada, a exceção de nulo pode ocorrer a qualquer descuido do programador.

# 8- Limites

Entender o limite de código de terceiros - SDKs.

Ao criar uma nova funcionalidade ou lidar com APIs terceiras, externas do contato do contexto do próprio time, fazer um teste de aprendizagem para entender os limites e o que os objetos dentro daquela API/SDK fazem ou validam. Ex: Validar métodos de criação são thread-safe por natureza na lib específica ou se o desenvolvedor deve criar esse controle diretamente.

Ao definir um comportamento esperado, siga com a implementação e garante que funcionará, e caso esse comportamento seja modificado em novas versões, fique claro o ponto de erro e de atualização que deverá ser feito no código. Assim, é possível seguir a implementação sem preocupações.

# 9- Testes unitários

Teste unitário por sua essência deve explicar o comportamento da unidade de código testada.

Criar funções utilitárias para manter linguagem de domínio.

Afirmação única: Dado → Quando → Então

Testes limpos → F.I.R.S.T

(Fast, Independent, Repeatable, Self-validating, Timely)

Testes devem ser rápidos, precisam não ter dependências de outros testes, devem ser repetidos em qualquer lugar, devem ter uma validação booleana para facilitar entendimento se o teste falhou ou passou.

# 10- Classes

Classes pequenas, devem ser pequenas devido a responsabilidade atrelada a classe.

O nome das classes afetam a quantidade de responsabilidade que vai ser atrelada a ela, escolha um bom nome.

Princípio da responsabilidade única → A classe deve ter apenas um motivo para mudar.

Separar um método grande ajuda a entender se devemos criar mais classes para métodos menores que utilizem os mesmo parâmetros, para que ao invés de passar diversos parâmetros, compartilhemos esses valores numa variável de instância da nova classe que será criada para juntar os comportamentos.

Open-Closed Principle → Aberto para extensão e fechadas para alteração. Exemplo do clean code:

Classe Sql → Select extends SQL, Insert extends SQL, Update extends SQL. 

O que muda é a classe que irá extender SQL e SQL adota um padrão de implementação onde todas as classes que extenderem utilizem o padrão.

Classe portifolio depende de TokyoStockExchange e TokyoStockExchange muda toda hora seu valor, para refatorar, cria-se uma interface StockExchange, TokyoStockExchange implementa a interface, e portifolio apenas consome o valor que a interface StockExchange retorna, de acordo com o tipo de implementação que Portifolio use, com ajuda da DIP(Dependency inversion principle).

# 11- Sistemas

Sistemas devem separar o processo de inicialização - construção de objetos e “conexões” entre dependências - da lógica em tempo de execução.

Projetos devem ser criados com analogia de cidades, primeiro se tem um vilarejo, depois a urbanização leva a uma cidade maior, e de acordo com a demanda, o sistema deve se adaptar e aumentar seus serviços. Para software, isso deve significar a refatoração necessária de acordo com a necessidade.

Todo projeto deve tentar seguir 4 pontos:

1- Testar todas funcionalidades críticas

2- Não possuir duplicação de código

3- O código expressar o propósito dele estar escrito daquela maneira

4- Minimizar quantidade de classes e métodos.

Utilizando esses pontos como um guia, é possível criar um sistema limpo.

# 12- Concorrência

Objetos são abstrações de procedimentos.

Threads são abstrações de agendamento.

Existem diversas maneiras de controlar métodos com diversas threads, usando locks direto no banco, @Transactional do Spring, controle de pool de threads, gerência de memória de cada thread.

Preciso criar mais projetos que trabalham com processamentos multi-thread para melhorar o meu entendimento básico de threads, no momento ainda sinto muito “cru” para entender o que o livro comenta.

Devemos utilizar threads para facilitar o processamento de dados, seja de I/O, input and output, ou processamento pesado. No java temos as threads básicas, que são custosas de instanciar e conseguem gerenciar alta carga de trabalho e as Virtual threads do Java 21 que ajudam no processamento I/O. 

# 13, 14, 15 - Refatorações de classes

Nesses capítulos foi mostrado exemplos práticos de refatorações seguindo todos os conceitos que o livro repassa.

# 16 - Code smells

DRY- Dont repeat yourself.

Código morto, por exemplo condições booleanas adicionadas durante um tempo que após tantas mudanças de fluxo, aquela condição não possue nenhum cenário onde seria verdadeira ou falsa.

Princípio da surpresa mínima: Se escolher fazer as coisas de um jeito, continue fazendo para os próximos fluxos, mas é necessário se atentar no momento da escolha.

Colocar booleanos como parâmetros de funções, é preferível passar um ENUM com um nome explicativo do que um true ou false num parâmetro, isso faz com que o desenvolvedor sem contexto perca tempo tentando entender o fluxo inteiro.

Numeros e palavras mágicas, sempre declarar variáveis com nomes descritivos a nível de classe para evitar adivinhação dentro do código.

Responsabilidade de código mal posicionada, refere-se a não saber onde colocar variáveis ou métodos para exemplificar a regra de negócio, caso isso aconteça, tente usar o princípio da surpresa mínima, onde seria o lugar mais lógico para colocar essa regra ? 

Preferir polimorfismo a excesso de if/else no código.

Encapsular condicionais em métodos com nome descritivo ao invés de true ou false.

Acoplamento de execução de métodos, utilizar variáveis para que a ordem da execução fique óbvia e não ser mudada no futuro.
