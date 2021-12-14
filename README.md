# Arquitetura Data Lake na AWS, Camada de Persistência dos dados(Armazenamento) no HDFS e para manipular os dados usaremos Apache HIVE e Apache HBase.
![1]
Essa será a arquitetura quando todo o projeto estiver pronto, mas irei dividir o projeto para não ficar muito grande então cada camada irei implementar e separar por projeto.
Camada de Persistência dos dados(Armazenamento).

Todos os scripts estarão em anexo.

## Iniciando o Projeto
Iremos criar uma camada de Persistência dos dados na construção do Data Lake de Projetos Anteriores e veremos em projetos finais a construção total de um Data Lake.

## Duas Decisões importantes!

![2]

## Persistência dos dados

A camada de armazenamento é responsável pela persistência dos dados e deve estar pronta para escalabilidade sempre que necessário.
Eu preciso de persistências dos dados, ou seja, eu preciso gravar os dados em disco? Quem tem que responder essa pergunta é o seu objetivo de negócio se você está criando uma arquitetura, construindo uma arquitetura de Data Lake para processamento de dados em tempo real eu preciso armazenar uma vez que estou processando e entregando os dados em tempo real? Imagine que você está coletando dados do twitter e você está fazendo analise de sentimento em tempo real e entregando isso para o tomador de decisão para montar uma estratégia de marketing ali naquele momento eu preciso guardar aqueles twitters? Uma vez que o meu objetivo é entregar a análise de sentimento para o tomador da decisão? Isso é algo que precisa ser definido na equipe de engenheiro de dados a equipe de data Science e a equipe de negócios porque você pode gravar os dados, ou seja, a persistência em disco isso tem custo eu preciso ter uma estrutura de armazenamento, e mesmo se eu tiver que armazenar preciso saber por quanto tempo vou manter esses dados armazenados e entra outras questões como segurança, backup e etc... A pergunta chave que tem que ser feita logo no início do projeto é exatamente eu preciso de persistência dos dados?
![3]
Escolher a mídia física qual tipo de disco que eu vou usar para gravar os dados nós temos que considerar algumas possibilidades primeira coisa é definir é o >
-AIOPS (Input/Output Operations Per Second) ou seja, quantidade de operações de entradas e saídas ou leitura e escrita por segundo essa definição é importante para escolher o tipo de mídia que será usada dentro da sua arquitetura de Data Lake, SSD, HDD no meu caso que estou usando o AWS eu posso fazer essa escolha eu tenho a opção de escolher SSD, HDD e no caso do AWS eles cobram valores diferente por tipo de mídia que você utiliza.
Se você tiver um IOPS muito baixo, ou seja, pouca operações por segundo um HDD deve atender sem maiores problemas se por outro lado você vai precisar de muito IOPS, ou seja, você terá muita alteração de entrada e saída, muita leitura e escrita no seu ambiente de armazenamento ai é importante considerar SSD. Você pode também fazer uma estrutura hibrida vc usa o SSD aonde o IOPS é alto e quando os dados ficarem apenas persistindo em disco, ou seja, você não precisa executa-lo com frequência utiliza o HDD não existe um único cenário vai sempre depender do objetivo final e do orçamento disponível para o projeto.
E tem outra opção o SAN (Storage Area Network) ou seja, um ambiente de armazenamento em rede é basicamente você utilizar um storage, várias empresas como AMC fornecem storage com grande conjuntos de discos, são máquinas gigantesca puramente para armazenamento e como se fosse um HD local mas na verdade toda a gravação é enviado pela rede normalmente com Fibra ótica. Veja como essas decisões vão impactar diretamente no orçamento do projeto

![4]

![5]

![6]


O HDFS serve exatamente para armazenamento e fim.

![7]

O Hive você consegue manipular os dados de forma estruturada e semi-estruturada enquanto que com HBase eu basicamente faço manipulação de dados não estruturado, ou seja, o Hive e o HBase pode ser complementares na minha arquitetura de Data Lake ao invés de entregar para o cientista de dados diretamente o HDFS para que ele colete os dados leia os dados por exemplo eu posso entregar ao cientista de dados uma interface no Hive e ele pode usar linguagem SQL por exemplo para coletar dados, manipular, fazer algum tipo de consulta de sumarização de agregação de soma e assim por diante ou então posso entregar ao cientista de dados à interface do HBase para que ele possa manipular os dados de maneira aleatória que aliás não pode ser feito no HDFS, ou seja, o HDFS é muito bom para ARMAZENAMENTO que fique bastante claro. Quando você quiser manipular dados, veja nesse caso manipular não é apenas processar os dados eu quero fazer consultas, algum tipo de agregação, sumarização isso é manipulação de dados e utilizamos o Hive e HBase.

![8]

![9]

![10]

![11]

![12]

Esses processos e realizado no Hive no momento que você submete um querie SQL todos esses componentes são executados temos o Driver que recebe a execução temos o Metastore que vai armazenar e consultar os metadados temos o Compilador que vai compilar a instrução o Otimizador que vai pegar plano de execução e vai gerar o grafo computacional temos o Executor que vai executar todo o processo e posso fazer isso tudo via linha de comando também utilizando uma interface oferecidas pelo apache Hive.

![13]

![14]

## Criando tabela no Hive e testando se foi para o MySQL

![15]

![16]

Repare que a tabela criada no Apache Hive, ou seja, o nome da tabela é um metadados quando você carrega os dados no apache Hive eles vão para o HDFS os dados, ok? Mas o nome da tabela ele é um metadados então isso é gravado em uma informação aqui no metastore.
## Apache HBASE

![17]

![18]

![19]

![20]

- Veja a relação do HBase com o HDFS do lado esquerdo nós temos o Data Consumer, ou seja, um analista consumindo os dados ou mesmo a aplicação que faz uma consulta direta ao HBase e do lado direito nós temos o Data Producer ou seja aquele que está produzindo os dados então posso trazer dados com apache Kafka com apache NiFi. Perceba que no caso do Data Consumer ele faz a leitura dos dados diretamente do HBase o Data Consumer não consulta o HDFS ele vai no HBase e por sua vez vai no HDFS fazer a consulta dos dados o HBase é um tipo de interface. A vantagem disso e que nós podemos utilizar as principais características ou as melhores característica tanto do HBase quando do HDFS e perceba que no Data Producer ele pode gravar os dados tanto no HDFS quanto no HBase dependendo do volume que você está trabalhando é mais interessante em questão de performance gravar os dados no HDFS e utilizar o HBase apenas para manipulação, mas depende do volume dos dados que você está trabalhando.

![21]

![22]

Perfeitamente configurado com 2 cluster slave !!! Agora podemos passar o bastão para o cientista de dados que vai vir aqui e realizar seu processo de análise!!!

Fim. Esse é o resultado final do nosso Data Lake, Criamos a Infraestrutura necessária na AWS com as instâncias com os Cluster e fomos adicionando as Camada a cada projeto, Aquisição de dados em Batch e Streaming, Camada de Mensagem com Kafka e Camada de Armazenamento e podemos adicionar de processamento com Spark, Flink, Storm e etc.
