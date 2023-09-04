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

* EMR é escalável e roda com Big Data sob demanda na nuvem. Portanto, você não precisa operar o Big Data o tempo todo, pois isso custa caro;

* Já vem pré-instalado para Hadoop, Spark, Hive e Tensorflow;

* O EMR é um EC2 primário que invoca outros EC2 no back-end como nós de trabalho secundários;

*  O EMR também usa o S3 como bucket, que por sua vez, estará como arquivos. Veja a arquitetura acima para enteder melhor;

*  O EMR/EC2 primário + EC2 nós secundários formam um cluster EMR;

*  Essa arquitetura é diferente de um computador comum: no seu laptop, os arquivos são armazenados de forma centralizada e única num mesmo dispositivo. No EMR, os arquivos são distribuídos e processados em partes ao mesmo tempo. O S3 permite que os arquivos (objetos) possam ser acessados em todos os cantos dentro do seu cluster;

* Pode-se integrar outros serviçso AWS, como Kinesis e Dynamondb.

