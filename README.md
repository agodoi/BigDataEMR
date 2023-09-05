# Big Data no Amazon EMR

Para iniciar essa prática, veja alguns conceitos importantes sobre o [Hadoop]
(https://docs.google.com/presentation/d/1yz8-VlhpG48SkhpnK44gLgMTD7PVMJ6ZEwv1hX3-Dto/edit?usp=sharing)


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/BigDataEMR/blob/main/imgs/BigDataAWS.jpg">
   <img alt="Arquitetura Inicial" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/BigDataEMR/blob/main/imgs/BigDataAWS.jpg)">
</picture>

O Amazon Elastic MapReduce (Amazon EMR) é um serviço de computação em nuvem oferecido pela AWS que permite processar e analisar grandes volumes de dados de maneira escalável e econômica. O EMR é projetado especificamente para processamento distribuído e análise de dados em larga escala usando o framework Apache Hadoop e outras ferramentas relacionadas.

A principal ideia por trás do Amazon EMR é permitir que as empresas processem e analisem grandes conjuntos de dados de maneira eficiente, sem a necessidade de investir em infraestrutura de hardware e configuração complexa. Ele permite executar tarefas como processamento de dados, análise de logs, mineração de dados, aprendizado de máquina, entre outras, em clusters de máquinas virtuais na nuvem.

Algumas das características e recursos do Amazon EMR incluem:

* Escalabilidade: O EMR pode ser dimensionado horizontalmente, permitindo adicionar ou remover facilmente nós ao cluster de processamento para lidar com variações no volume de dados e nos requisitos de processamento.

* Suporte a Frameworks: Além do Hadoop, o EMR suporta uma variedade de frameworks e ferramentas populares, como Apache Spark, Apache Hive, Apache Pig, Presto e mais, que facilitam a análise e processamento de dados.

* Gerenciamento Simplificado: O EMR gerencia automaticamente tarefas como provisionamento, configuração e monitoramento de clusters, o que reduz a carga administrativa.

* Integração com Ecossistema AWS: O serviço se integra perfeitamente com outros serviços da AWS, como o Amazon S3 para armazenamento de dados, o Amazon Redshift para análise de dados e o AWS Glue para transformação de dados.

* Modelo de Preços Flexível: O Amazon EMR oferece opções de instâncias sob demanda, reservadas e spot, permitindo escolher o modelo de preços mais adequado às necessidades e ao orçamento da organização.

* Segurança: O EMR oferece recursos de segurança que permitem proteger os dados durante o processamento, incluindo a integração com o AWS Identity and Access Management (IAM), além de suportar a criptografia de dados em repouso e em trânsito.

* Ferramentas de Monitoramento e Diagnóstico: O EMR oferece ferramentas para monitorar a utilização do cluster, visualizar logs e diagnosticar problemas de desempenho.

## Dicas importantes sobre EMR:

1) EMR é escalável e roda com Big Data sob demanda na nuvem. Portanto, você não precisa operar o Big Data o tempo todo, pois isso custa caro;

2) Já vem pré-instalado para Hadoop, Spark, Hive e Tensorflow;

3) O EMR é um EC2 primário que invoca outros EC2 no back-end como nós de trabalho secundários;

4) O EMR também usa o S3 como bucket, que por sua vez, estará como arquivos. Veja a arquitetura acima para enteder melhor;

5) O EMR/EC2 primário + EC2 nós secundários formam um cluster EMR;

6) Essa arquitetura é diferente de um computador comum: no seu laptop, os arquivos são armazenados de forma centralizada e única num mesmo dispositivo. No EMR, os arquivos são distribuídos e processados em partes ao mesmo tempo. O S3 permite que os arquivos (objetos) possam ser acessados em todos os cantos dentro do seu cluster;

7) Pode-se integrar outros serviçso AWS, como Kinesis e Dynamondb.

8) Grupo de Instância Primário (Master Instance Group): é o grupo principal de um cluster EMR. Este grupo é responsável por coordenar o cluster e gerenciar os recursos do cluster, como a distribuição de tarefas e a comunicação entre os nós. Geralmente, o nó mestre não executa tarefas de processamento de dados, mas desempenha um papel crucial na administração do cluster EMR. Normalmente, você tem apenas um grupo de instância principal em um cluster EMR.

9) Grupo de Instância Núcleo (Core Instance Group): são usados para executar tarefas de processamento de dados no cluster EMR. Os nós do grupo de instância núcleo são onde os dados são armazenados e processados. Esses nós têm acesso aos dados armazenados no Hadoop Distributed File System (HDFS) do cluster e executam tarefas MapReduce, Spark ou outras tarefas de processamento de dados. Você pode ter um ou mais grupos de instância núcleo em um cluster EMR, dependendo da capacidade de processamento e armazenamento necessária para o seu trabalho.


# Passo-01: Criando um cluster EMR

a) Dentro do console AWS, vá na lupa de busca, e digite **EMR** para puxar esse serviço.

b) Clique no botão laranja **Criar cluster**

c) Em **Nome**, coloque o nome desejado, vou sugerir **EMR-BigData-01**. Em **Versão** deixa como está. 

d) Em **Pacotes de aplicativos**, escolha **Spark**.

e) Em **Configuração do cluster**, deixe em **Grupos de instâncias**.

e.1) Em **Grupos de instâncias**, na opção **Primário**, deixe como está. Em **Núcleo**, também, deixe como está. Mas leia os itens **(8)** e **(9)** do **Dicas importantes sobre EMR** para entender quais os papeis de cada EC2 sendo instanciado.

e.2) Em **Tarefa 1 de 1**, coloque o nome **BigData-Tarefa01**. Isso é a sua primeira tarefa sendo criada. Uma tarefa equivale a um processamento de Big Data.

f) Em **Opções de provisionamento e escalabilidade do cluster** selecione a opção **Usar escalabilidade gerenciada pelo EMR**. Não precisa mudar mais nada nessa opção de configuração, mas verifique quantos cluster mínimos e máximos você terá e novamentem, reforce seu conceito olhando os itens **(8)** e **(9)** do **Dicas importantes sobre EMR** para entender o que são esses números e qual a diferença entre **Grupo de Instância Primário** e **Núcleo**. 

g) Em **Redes**, crie uma VPC nova chamada **VPC-BigData-Aula01**. No momento de criar a VPC, clicando em **Criar VPC**, marque **Somente VPC** e coloque o nome **VPC-BigData-Aula01**. O IPV4 pode deixar a subrede genérica **10.0.0.0/24**.

g.1) Agora, de volta na opção **Redes**, clique na bolinha de atualizar que vai carregar a **VPC-BigData-Aula01** e deixe-a marcada e clique em **Escolher**.

h) Em **Subrede**, você vai ter que criar uma nova, pois a **default** ficou sem usar. Já que você criou uma nova VPC, terá que criar uma nova subrede. Clique em **Criar subrede**, depois aponte para VPC que você acabou de criar, coloque o nome da subrede de **Sub_PublicaBigData**, aponte para **us-east-1a**, e no IPV4, coloque a mesma faixa de endereço **10.0.0.0/24**.

