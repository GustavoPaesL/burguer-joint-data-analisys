# Dashboard de análise de vendas

Dashboard interativo para a atividade de Fundamentos de IA e Análise de Dados. A interface usa HTML, CSS e JavaScript puro, com Chart.js para os gráficos e Papa Parse para a leitura de CSV.

## Como abrir

Abra [`public/index.html`](public/index.html) no navegador. A amostra já está incorporada no JavaScript, então a demonstração aparece imediatamente, sem `fetch` e sem servidor local.

As bibliotecas são carregadas por CDN e, portanto, é necessário ter internet para visualizar os gráficos e importar/exportar CSV pela interface. O restante da página continua sendo um arquivo estático.

## Dados

Ao abrir a página, o dashboard usa a base original `vendas_hamburgueria.csv`, com 1.367 registros de vendas. Ela foi incorporada ao JavaScript para funcionar também ao abrir `public/index.html` diretamente. A interface identifica essa origem como **Base original**. Use **Importar CSV** para selecionar uma base própria; a leitura ocorre localmente no navegador e não envia o arquivo para um servidor.

O arquivo `public/dados-exemplo.csv` continua disponível como exemplo alternativo com 200 registros fictícios distribuídos por 12 meses.

Colunas esperadas: `id_venda`, `data` (AAAA-MM-DD), `produto`, `categoria`, `regiao`, `quantidade` e `preco_unitario`. Linhas com ID ausente, data inválida ou quantidade/preço inválidos são ignoradas. Quantidade e preço negativos também são descartados. Categoria, produto ou região vazios recebem `Não informado`/`Não informada`.

O projeto preserva o arquivo original `vendas_hamburgueria.csv`. Como ele não possui `regiao`, o dashboard atribui regiões fictícias de forma determinística, usando o ID da venda em ciclo entre `Centro`, `Zona Norte`, `Zona Sul`, `Zona Leste` e `Zona Oeste`. Essa adaptação permite usar o gráfico e o filtro regional sem alterar a base original. A cópia derivada com essa coluna está em `public/vendas_hamburgueria_com_regiao.csv`.

## Fórmulas

- Faturamento da linha = `quantidade × preco_unitario`.
- Faturamento total = soma do faturamento das linhas após os filtros.
- Quantidade de vendas = quantidade de `id_venda` únicos.
- Unidades vendidas = soma de `quantidade`.
- Ticket médio = faturamento total ÷ quantidade de vendas; quando não há vendas, o resultado é R$ 0,00.

Filtros de período, categoria e região atualizam simultaneamente os cartões, os gráficos e a tabela. A pesquisa da tabela considera todos os campos visíveis, e os cabeçalhos permitem ordenação. O botão **Exportar CSV** baixa somente os registros que estão filtrados e pesquisados.

O bloco **Resumo inteligente** recalcula, conforme os filtros, o faturamento, a categoria e o produto líderes, a evolução entre o primeiro e o último mês disponível e uma comparação de meses atípicos. A detecção de variação usa 1,5 desvio-padrão da média mensal e não afirma causalidade. Com menos de três meses, o painel informa que não há dados suficientes para uma leitura de anomalias confiável.

## Publicação no Render

1. Envie o projeto para um repositório no GitHub.
2. No Render, crie um novo serviço do tipo **Static Site**.
3. Selecione o repositório e a branch que contém o projeto.
4. Deixe **Root Directory** vazio.
5. Use `echo "Site pronto"` como **Build Command**.
6. Use `public` como **Publish Directory**.
7. Publique o serviço.

Não há backend, banco de dados ou etapa de compilação.

## Verificação realizada

Foi conferida a base original com 1.367 registros: faturamento total aproximado de R$ 36.506,00, 1.367 vendas, 2.060 unidades e ticket médio de R$ 26,71 (arredondado na exibição). Também foram revisados os fluxos no código para importação, filtros combinados, limpeza, exportação e estado sem resultados. A aparência responsiva foi implementada para telas grandes e pequenas.

Nesta sessão não houve navegador gráfico automatizado disponível; por isso, a inspeção visual e os cliques reais no navegador não puderam ser executados aqui. Abra o arquivo no navegador com internet para a verificação final de Chart.js e Papa Parse.
