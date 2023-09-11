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

1) EMR é escalável e roda com Big Data sob demanda na nuvem. Portanto, você não precisa operar o Big Data o tempo todo, pois isso custa caro. Ligue e desligue seu EMR;

2) Já vem pré-instalado para Hadoop, Spark, Hive e Tensorflow;

3) O EMR é um EC2 primário que invoca outros EC2 no back-end como nós de trabalho secundários;

4) O EMR também usa o S3 como bucket, que por sua vez, armazenará arquivos. Veja a arquitetura acima para enteder melhor;

5) O EMR/EC2 primário + EC2 nós secundários formam um cluster AWS EMR;

6) Essa arquitetura é diferente de um computador comum: no seu laptop, os arquivos são armazenados de forma centralizada e única num mesmo dispositivo. No EMR, os arquivos são distribuídos e processados em partes ao mesmo tempo. O S3 permite que os arquivos (objetos) possam ser acessados em todos os cantos dentro do seu cluster;

7) Pode-se integrar outros serviçso AWS, como Kinesis e Dynamondb.

8) Grupo de Instância Primário (Master Instance Group): é o grupo principal de um cluster EMR. Este grupo é responsável por coordenar o cluster e gerenciar os recursos do cluster, como a distribuição de tarefas e a comunicação entre os nós. Geralmente, o nó mestre não executa tarefas de processamento de dados, mas desempenha um papel crucial na administração do cluster EMR. Normalmente, você tem apenas um grupo de instância principal em um cluster EMR.

9) Grupo de Instância Núcleo (Core Instance Group): são usados para executar tarefas de processamento de dados no cluster EMR. Os nós do grupo de instância núcleo são onde os dados são armazenados e processados. Esses nós têm acesso aos dados armazenados no Hadoop Distributed File System (HDFS) do cluster e executam tarefas MapReduce, Spark ou outras tarefas de processamento de dados. Você pode ter um ou mais grupos de instância núcleo em um cluster EMR, dependendo da capacidade de processamento e armazenamento necessária para o seu trabalho. O Núcleo é o mesmo que um EC2 secundário. Ele obedece o EC2 primário.
    
10) Instâncias Tarefas (Task Instances): são usadas para trabalhos temporários, que não exigem armazenamento persistente. Elas não fazem parte do sistema de armazenamento do cluster, ou seja, não mantêm cópias de dados no HDFS. As instâncias task são usadas principalmente para aumentar a capacidade de processamento temporariamente, e elas podem ser adicionadas ou removidas conforme necessário. Por não armazenarem dados persistentes, as instâncias task são mais efêmeras e são usadas para cargas de trabalho que podem ser distribuídas em várias instâncias temporárias. Elas são ideais para trabalhos de processamento em lote e tarefas de curta duração.


# Passo-01: Criando um cluster EMR

a) Dentro do console AWS, vá na lupa de busca, e digite **EMR** para puxar esse serviço.

b) Clique no botão laranja **Criar cluster**

c) Em **Nome**, coloque o nome desejado, vou sugerir **EMR-BigData-01**. Em **Versão** deixa como está. 

d) Em **Pacotes de aplicativos**, deixe em **Spark**.

e) Em **Configuração do cluster**, deixe em **Grupos de instâncias**.

e.1) Em **Grupos de instâncias**, na opção **Primário**, deixe como está. Em **Núcleo**, também, deixe como está. Mas leia os itens **(8)** e **(9)** do **Dicas importantes sobre EMR** para entender quais os papeis de cada EC2 sendo instanciado.

e.2) Em **Tarefa 1 de 1**, coloque o nome **BigData-Tarefa01**. Esse nome vai parar no EC2 **Primário**, isto é, esse nome é de um EC2 que o **Primário** vai coordenar. Uma tarefa equivale a um processamento exclusivo de Big Data.

f) Em **Opções de provisionamento e escalabilidade do cluster** selecione a opção **Definir o tamanho do cluster manualmente**. Não precisa mudar mais nada nessa opção de configuração, mas verifique quantos **Nucleos** e  máximos você terá e novamente, reforce seu conceito olhando os itens **(8)**, **(9)** e **(10)** do **Dicas importantes sobre EMR** para entender o que são esses números e qual a diferença entre **Grupo de Instância Primário**, **Núcleo** e **Tarefas**.    

g) Em **Redes**, crie uma VPC nova chamada **VPC-BigData-Aula01**. No momento de criar a VPC, clicando em **Criar VPC**, marque **Somente VPC** e coloque o nome **VPC-BigData-Aula01**. O IPV4 pode deixar a subrede genérica **10.0.0.0/24**.

g.1) Agora, de volta na opção **Redes**, clique na bolinha de atualizar que vai carregar a **VPC-BigData-Aula01** e deixe-a marcada e clique em **Escolher**.

h) Em **Subrede**, você vai ter que criar uma nova, pois a **default** ficou sem usar. Já que você criou uma nova VPC, terá que criar uma nova subrede. Clique em **Criar subrede**, depois aponte para VPC que você acabou de criar, coloque o nome da subrede de **Sub_PublicaBigData**, aponte para **us-east-1a**, e no IPV4, coloque a mesma faixa de endereço **10.0.0.0/24**.

i) Em **Término do cluster**, deixe em **Encerrar o cluster automaticamente após 1h**, assim, esse trem será desligado quando encerrar essa atividade, ou seu dinheiro irá pro espaço.

j) Em **Configuração de segurança e par de chaves do EC2 - opcional**, vá na linha de **Par de chaves do Amazon EC2 para o SSH do cluster** e clique em **Navegar** e pegue uma chave PEM que você já tenha criado. Mas anote isso para não esquecer depois ou terá que fazer tudo novamente. Minha sugestão é que você reaproveite a chave **FlaskServerUbuntu**

k) Em **Perfil de serviço do Amazon EMR**, vá em **Função de serviço** e escolha **EMR_AutoScalling_DefaultRole**.

l) Em **Perfil de instância do EC2 para o Amazon EMR**, vá em **Perfil de instância** e escolha **EMR_EC2_DefaultRole**.

m) Clique no botão amarelo **Criar cluster**.

n) Agora volte na busca do AWS, digite **EMR** e você verá que seu cluster **EMR-BigData-01** está em fase de preparação. Isso é normal. Precisa aguardar alguns minutos.

# Passo-02: Fazendo download de um "grande arquivo" (não é tão grande, mas serve de exemplo)

a) Vá em [StackOverflow](https://insights.stackoverflow.com/survey) que é um site que hospeda os resultados da Pesquisa de Desenvolvedores do Stack Overflow, uma pesquisa anual amplamente reconhecida que coleta dados sobre a comunidade de desenvolvedores de software em todo o mundo. 

Faça o download do arquivo CSV.zip do ano passado. Esse arquivo vai representar nosso Big Data. Descompacte esse arquivo zip e você vai encontrar outros 4 arquivos (um *.TXT, um *.PDF e dois *.CSV). Abra o arquivo de maior tamanho em Excel e terá mais de 700mil linhas de dados. Isso é um Big Data já.

b) Deixe esses 4 arquivos aí no seu computador que já vamos fazer um upload dele em um serviço de armazenamento S3 da AWS.

# Passo-03: Criando um bucket S3

a) No busca do AWS, digite **S3**. Note que já existe um **aws-logs-xxx-us_east-1** na listagem do seu bucket S3. Isso é normal quando você cria um serviço EMR. Não vamos usar esse S3 bucket por enquanto e sim, criar um novo.

b) Clique no botão laranja para criar um novo bucket e nomeie-o da forma que achar amigável, por exemplo, **s3-bigdata-01**, mas não tente usar esse nome porque ele já foi usado e todos os nomes de S3 na AWS devem ser inéditos e exclusivos mundialmente. Só lembrando, que o nome do bucket não pode ter letras maiúsculas e nem underline.

c) Deixe-o **bloquear todo o acesso público**, isso significa que o seu S3 não estará visível na Internet. Isso é uma medida de segurança importante para garantir que os dados armazenados no seu bucket do S3 não fiquem expostos acidentalmente ao público. É sempre recomendável seguir as práticas recomendadas de segurança ao configurar buckets no Amazon S3 e garantir que apenas os recursos e usuários autorizados tenham acesso aos dados armazenados.

d) Em **Versionamento de bucket** deixe como **Ativar**. Isso significa que você poderá ter várias versões de seu processamento de Big Data e recuperar versões seguras de resultados confiáveis do seu processamento.

e) Não precisa mexer em mais nenhum outra configuração, então, basta clicar no botão laranja para criar.

f) Retorne na sua lista de buckets S3, clique no link azul do bucket que acabou de criar, e crie **uma nova pasta** e coloque o nome de **emr-fonte-dados**. Você é livre para criar o nome que quiser.

g) Em **Criptografia no lado do servidor** clique em **Especificar chave de criptografia** e depois **Usar configurações de bucket para criptografia padrão**.

h) Clique no botão laranja para confirmar.

i) Retorne à pasta que acabou de criar, clique no link azul dela, e faça o upload do arquivo *CSV de maior tamanho que puxou no Passo-02. Caso tenha algum espaço em branco no seu arquivo CSV, remova-o ou substitua-o com underline ou traço. Clique em carregar.

# Passo-04: Escrevendo o código Spark de processamento
```
# Importando as bibliotecas necessárias do PySpark
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

# Definindo o caminho do arquivo de dados de entrada no S3
S3_DATA_SOURCE_PATH = 's3://... *.csv'    

#vá no seu respectivo bucket S3, copie o URI e cole aqui acima
#exemplo: s3://s3-bigdata-01/emr-fonte-dados/
#e copie o nome do bucket *.csv também e cole no final do path

# Definindo o caminho para onde os dados processados serão escritos no S3
S3_DATA_OUTPUT_PATH = 's3://.../data-output'    #estamos criando um dir de saída de dados. No lugar dos ... cole a memsa URI do bucket S3 recém criado

# Função principal do programa
def main():
    # Criando uma sessão Spark com um nome de aplicativo
    spark = SparkSession.builder.appName('AWSMapReduceDemoApp').getOrCreate()
    
    # Lendo o arquivo CSV de entrada e inferindo o cabeçalho
    all_data = spark.read.csv(S3_DATA_SOURCE_PATH, header=True)
    
    # Imprimindo o número total de registros no arquivo de dados de entrada
    print('Numero total de gravacoes na fonte de dados: %s' % all_data.count())
    
    # Filtrando os dados para selecionar profissionais dos Estados Unidos que trabalham mais de 45 horas por semana
    selected_data = all_data.where((col('Country') == 'United States') & (col('WorkWeekHrs') > 45))
    
    # Imprimindo o número de profissionais selecionados
    print('O numero de profissionais que trabalham mais que 45 horas por semana nos EUA: %s' % selected_data.count())
    
    # Escrevendo os dados selecionados no formato Parquet no caminho de saída no S3
    selected_data.write.mode('overwrite').parquet(S3_DATA_OUTPUT_PATH)
    
    # Imprimindo uma mensagem de sucesso
    print('Dados selecionados foram salvos com sucesso no s3: %s' % S3_DATA_OUTPUT_PATH)

# Verifica se o código está sendo executado como um script independente
if __name__ == '__main__':
    # Chama a função principal para iniciar o processamento
    main()
```

# Passo-05: Formato Parquet

Pronuncia-se "parquê" é um formato novo exclusivo para Big Data. Quais são suas características e vantagens?

- É um formato de armazenamento em coluna (ao invés de linha como acontece nos formatos de armazenamento tradicionais dos bancos de dados);
- Por isso, fornece otimizações para acelerar consultas;
- Foi criado para suportar compressão;
- O arquivo é dividido em dados e metadados;
- Está presente no Spark, Hive e no ecossistema Hadoop em geral.

## Exemplo:
| Formato | Espaço Utilizado | Tempo Excecução | 
CSV      |   2TB   | 472seg   |
Parquet   |   260GB   | 13,56seg   |

