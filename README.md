📘 README - Desafio Projeto

📌 Objetivo

Este desafio tem como objetivo a integração do Power BI com um banco de dados MySQL hospedado na nuvem, além da transformação e modelagem dos dados para futura análise.

📋 Etapas do Desafio

1️⃣ Criação da Instância do Banco de Dados

Originalmente, a proposta era utilizar o Microsoft Azure. No entanto, devido a problemas na validação do telefone, foi utilizada a plataforma AWS como alternativa.

2️⃣ Criação do Banco de Dados

O banco de dados foi criado com base nos arquivos disponíveis no GitHub.

3️⃣ Integração com o Power BI

Foi estabelecida a conexão entre o Power BI e o MySQL hospedado na AWS.

4️⃣ Transformação de Dados no Power BI

Várias transformações foram realizadas para garantir a qualidade dos dados e facilitar a análise.

🔄 Diretrizes para a Transformaçāo dos Dados

✅ Verificação de Cabeçalhos e Tipos de Dados

Revisão dos tipos de dados para garantir consistência e correta manipulação.

✅ Conversão de Valores Monetários

Todos os valores monetários foram convertidos para o tipo double para maior precisão.

✅ Tratamento de Valores Nulos

Analisamos e tratamos os valores ausentes.

✅ Análise dos Employees com Super_ssn Nulo

Identificamos que alguns employees sem Super_ssn podem ser gerentes e verificamos se havia algum colaborador sem gerente associado.

✅ Verificação de Departamentos Sem Gerente

Caso algum departamento estivesse sem gerente, assumimos que os dados estavam disponíveis e preenchemos as lacunas.

✅ Verificação do Número de Horas nos Projetos

Analisamos a quantidade de horas atribuídas a cada projeto para garantir a coerência dos dados.

✅ Separação de Colunas Complexas

Foram separadas colunas que continham dados concatenados para facilitar a análise.

✅ Mescla das Tabelas Employee e Department

Criamos uma nova tabela employee contendo o nome dos departamentos associados aos colaboradores.

Utilizamos um merge entre employee e department, garantindo a correta junção das informações.

✅ Remoção de Colunas Desnecessárias

Eliminamos colunas que não seriam utilizadas no relatório final.

✅ Junção dos Colaboradores com seus Respectivos Gerentes

A relação entre empregados e gerentes foi realizada por meio de uma consulta SQL ou mesclagem de tabelas no Power BI.

✅ Mesclagem de Nome e Sobrenome

Criamos uma única coluna com o nome completo dos colaboradores.

✅ Mesclagem dos Nomes de Departamentos e Localização

Cada combinação departamento-localização se tornou única para auxiliar na criação do modelo estrela.

Utilizamos a função merge em vez de assign, pois:

A mesclagem combina valores de duas colunas em uma única.

Assegura a unicidade das combinações.

Simplifica o modelo de dados, evitando duplicidade.

Facilita a criação de um modelo estrela eficiente.

✅ Agrupamento de Dados: Contagem de Colaboradores por Gerente

🔹 Power Query (Linguagem M):

GerenteNome = Table.AddColumn(Fonte, "GerenteNome", each 
    try Record.Field(
        Table.SelectRows(Fonte, each [Ssn] = [Super_ssn]){0}, "Full name"
    ) 
    otherwise null
)

Agrupado = Table.Group(GerenteNome, {"GerenteNome"}, {{"Colaboradores", each Table.RowCount(_), Int64.Type}})

🔹 DAX (Power BI):

GerenteNome = LOOKUPVALUE(
    'merge1'[Full name], 
    'merge1'[Ssn], 'merge1'[Super_ssn]
)

ColaboradoresPorGerente = CALCULATE(
    COUNTROWS('merge1'), 
    ALLEXCEPT('merge1', 'merge1'[GerenteNome])
)

✅ Remoção Final de Colunas Inúteis

Eliminamos as colunas que não seriam utilizadas no modelo final.

🎯 Conclusão

Com essas transformações, os dados foram limpos, estruturados e modelados de maneira eficiente, permitindo a análise futura com um modelo de dados otimizado no Power BI.

📌 Autor: Marcelo Orso📌 Data: 01/2025📌 Tecnologias Utilizadas: Power BI, MySQL (AWS), Power Query, DAX
