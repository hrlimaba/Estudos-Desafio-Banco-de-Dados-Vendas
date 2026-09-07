# Estudos-Desafio-Banco-de-Dados-Vendas
Projeto que apresenta uma solução analítica em Power BI, integrando MySQL e Azure com tratamento, validação e modelagem de dados utilizando o Power Query, convertendo informações brutas em indicadores estratégicos para decisões orientadas a resultados.

1. Apresentação do Desafio

Criação de um relatório no Power BI com a utilização de um banco de dados Azure_Company (teste) que foi desenvolvido e hospedado na plataforma Azure da Microsoft. A base foi estruturada e populada por meio dos scripts disponibilizados no desafio, presente nos arquivos script_bd_company.sql (estrutura da tabela) e insercao_de_dados_e_queries_SQL (conteúdo), utilizando MySQL e seus comandos para criação das tabelas, inserção e consulta dos dados. 
O projeto tem o objetivo de integrar o MySQL, banco de dados da Azure e o Power BI, utilizando uma conexão remota para a alimentação dos dados e o Power Query para o tratamento e correção das anomalias de acordo com as diretrizes propostas no desafio. Após a preparação dos dados, as informações foram organizadas e apresentadas em um relatório visual simples, em duas páginas, utilizando os recursos e ferramentas gráficas disponíveis no Power BI.


2. Desenvolvimento dos tópicos exigidos no desafio

a) Criar instância na Azure para MySQL

Logon: company
Banco de Dados: azure_company
Grupo de Recursos: teste
Servidor: desafio-dio-hrlima.mysql.database.azure.com
Terminal para Comandos MySQL utilizando o Cloud Shell 

b) Criar banco de dados com base disponível no github;
Banco de Dados: azure_company

c) Fazer a integração do Power BI com MySQL no Azure

Problema Encontrado: Scrips das tabelas foram aceitas mas os que apresentavam as informações para o seu conteúdo apresentavam erros e após logon na base de dados MySQL no Power BI, a base chegava vazia.

Solução: Após êxito na conexão e chegada dos tabelas com os cabeçalhos intactos com as informações zeradas, por não conseguir resolver a inserção das informações das tabelas no Terminal Cloud Shell no Azure, tentei e consegui transformar as informações contidas no script para arquivo em excel, alimentando o banco de dados para finalmente dar prosseguimento aos trabalhos de análise e transformação de dados e apresenta-los no relatório criado no Power BI.

Importância do Fato: Destaco que toda a criação do banco de dados e a importação foram realizados e a dificuldade na inserção do script contendo as informações das tabelas que foram devidamente criadas na plataforma Azure não impediram que fosse encontrada uma solução que no caso foi de transformar os dados do scrpt em formato excel e em seguida sendo importada as informações e para posteriormente serem tratadas e transformadas em informações para tomada de decisão. 

Exemplo de comandos usados no terminal - Cloud Shell disponível no Azure para consulta/verificação do Banco de Dados:

mysql -h desafio-dio-hrlima.mysql.database.azure.com -u company -p;
show databases;
use azure_company;
desc employee ... demais tabelas, dentre outros comandos;

d) Verificar problemas na base a fim de realizar a transformação para melhor utilização dos dados;

d1) Verificados os cabeçalhos, tipos de dados, os valores monetários, a existência de nulos, foram feitos transformações de dados, mesclagens e foram removidas as informações desnecessárias das novas tabelas criadas para a criação dos gráficos, atendendo as diretrizes e métricas do desafio.

d2) Foram eles neste Desafio:
//ABSTRAÇÃO 1: Na tabela azure_company employee, após análise, foi verificado que o funcionário James E Borg não tinha informações no campo Super_ssn, após consulta, foi informado pela empresa que ele seria gerente;

//Formatação das Datas: Nas tabelas azure_company departamento, azure_company dependente e azure_company employee as informações foram importadas com números que foram formatadas para a estrutura de dados no formato data (dd/mm/yyyy);

//Foi modificado o título da coluna na tabela azure_company works_on de Pno para Pnumber com o objetivo de obter um relacionamento com a tabela azure_company project para a criação do gráfico entre projetos x horas trabalhadas; 

//Foi criada uma medida para a determinação da quantidade de funcionários existentes na empresa com o código a seguir:
Total Integrantes = DISTINCTCOUNT('azure_company employee'[Employee])

d3) Análise e discussão das funções MESCLAR CONSULTA X ACRESCENTAR CONSULTA no Power BI

MESCLAR (MERGE) e ACRESCENTAR (Append): MESCLAR, faz a união de colunas entre duas tabelas diferentes com base em uma coluna em comum - como um Join no SQL. O resultado é uma tabelaque se apresentará mais ampla, com mais colunas, podendo ser excluídas as colunas desnecessárias. ACRESCENTAR, empilha os dados de duas ou mais tabelas com estrutura semelhante, uma em baixo da outra - como um Union.

Aplicação do MESCLAR no Desafio - Justificativa:
Assim como nas demais tabelas criadas com a função MESCLAR, foram utilizadas as tabelas onde se encontravam informações necessárias, foram fundidas, eliminadas as colunas desnecessárias e com a nova tabela criada, tornou-se possível a criação dos gráficos que servirão de base para análise e posterior tomada de decisões gerenciais dentro da organização.


No Desafio: Descrição do passo a passo da mescla entre as tabelas azure_company.departamento (departamento) e azure_company.employee (employee): 

I. Identificação das tabelas: foram selecionadas as tabelas azure_company.departamento e azure_company.employee;
II. Criação de uma nova consulta: foi utilizada a opção “Mesclar consultas como novas”, conforme solicitado;
III. Definição da chave de mesclagem: foi identificada a coluna Dnumber, presente nas duas tabelas, para realizar a combinação dos dados;
IV. Exclusão de coluna duplicada: após a mesclagem, uma das colunas Dnumber foi removida para evitar duplicidade;
V. Limpeza dos dados: foram realizadas as demais exclusões necessárias, mantendo somente as informações relevantes;
VI. Resultado final: a tabela Nova_Tabela_Employee foi concluída e ficou pronta para utilização na criação do gráfico da quantidade de funcionários por departamento.


//Foram realizadas 4 junções (mesclas) e 1 duplicação de tabela, sendo criadas 5 novas tabelas com o objetivo de determinar os itens exigidos neste desafio. 
São elas: Nova_Tabela_Employee, Nova_Tabela_Employee-Dependent, Nova_Tabela_Employee-Projetos, Nova_Tabela_Dept-Locations e Nova_Tabela-Manager;

//ABSTRAÇÃO 2 - MESCLA 1: Neste desafio, foi determinado que a coluna (Ssn) presente na tabela azure_company dependentes seria um item de identificação dos colaboradores principais da empresa com o intuito de associá-los aos seus respectivos dependentes. Com isso foram mescladas as tabelas azure_company dependentes com a azure_company employees, sendo portanto criada a nova tabela Nova_Tabela_Employee-Dependent utilizada para a criação do gráfico Colaboradores x Nome Dependentes;

//MESCLA 2 - Foram mescladas as tabelas azure_company departament e azure_company dept_location, sendo criada a nova tabela Nova_Tabela_Dept-Location para que fosse possível localizar e ilustra no mapa as regiões onde a empresa atua e seus departamentos.

//MESCLA 3 - Foram mescladas as tabelas azure_company departament e a azure_company employee, sendo criada a nova tabela Nova_Tabela_Employee para que fosse possível ser produzida o  treemap que ilustra a quantidade de colaboradores existente por departamento;

//MESCLA 4 - Foram mescladas as tabelas azure_company works_on e a azure_company employee, sendo criada a nova tabela Nova_Tabela_Employee-Projetos para que fosse possível demonstrar de forma gráfica a quantidade de projetos que cada colaborador está executando;

//DUPLICAÇÃO - A tabela azure_company employee foi duplicada para utilização das informações presentes nas colunas Employee e Super_ssn (esta última adotada como um id do gerente ligado ao seu colaborador). Nesta nova tabela denominada Nova_Tabela_Employee-Manager foi acrescentada uma nova coluna com o título Manager que contempla o nome do gerente. Esta tabela foi de grande utilidade para atendimento a solicitação proposta de informar os gerentes e seus respectivos colaboradores.


3. Criação do Relatório

Primeira Página:

Objetivo - Pagina destinada à informações sobre a quantidade de funcionários, tempo em horas utilizado na execução dos projetos e a quantidade de projetos sendo executados até o momento.
 
//CARD 1: Apresentado a informação sobre a quantidade de funcionários da empresa;

//CARD 2: Apresentado a informação sobre as horas trabalhadas pelos funcionários nos projetos da empresa sob sua responsabilidade;

//CARD 3: Apresentado a quantidade de projetos da empresa;

//GRÁFICO DE COLUNAS EMPILHADAS: Apresentado para cada colaborador a quantidade de projetos sob a sua responsabilidade;

//GRÁFICO DE ROSCA: De uma forma mais gráfica, foram apresentadas as horas dispendidas em cada um dos projetos com seus respectivos percentuais dentro de um período global e as nomenclaturas na legenda;

//GRÁFICO DE BARRA CLUSTERIZADO: Apresentado a remuneração de cada colaborador em função do seu cargo e responsabilidade em cada projeto.

//SCROLLER: Destaque para os projetos e horas trabalhadas em cada um deles.


Segunda Página:

//TABELA: Tabela que demonstra os gerentes, id e seus respectivos colaboradores sob sua gestão;

//GRÁFICO DE PIZZA: Informação sobre os colaboradores que apresentam dependentes, a quantidade e em percentuais com relação a quantidade total existente;

//GRÁFICO DE BARRAS CLUSTERIZADO: Foi demonstrado informações sobre a quantidade e os segmentos dos departamentos;

//MAPA: Localização no mapa dos departamentos por região;

//TREEMAP: Foi informada a quantidade de colaboradores existentes por departamento.
