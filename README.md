<h1>Concessionária de Carros</h1>

Este projeto tem como objetivo a análise de preços de veículos e suas vendas de uma concessionária.

<hr>

<h2><a href="https://app.powerbi.com/view?r=eyJrIjoiODA1NjZhOTQtOTkwZi00NzA0LWE4MmQtYThjYTMxMWI3NTFkIiwidCI6IjBjM2IyYzljLWVlYTctNDJlZi04YTYzLTcwOWIyNjU5NzYxOCJ9">Acesse o Dashboard aqui!</h2>

<h2>Entendendo a Base de Dados</h2>

<b>Arquivo:</b> Arquivo simples delimitado por vírgula (car_prices.cs)

<b>Principais colunas:</b>

- <b>ID:</b>  Identificador único do veículo

- <b>make:</b>  Fabricante do veículo

- <b>model:</b>  Modelo do veículo

- <b>year:</b> Ano de fabricação

- <b>color:</b> Cor do veículo

- <b>sellingprice:</b> Preço de venda

- <b>expectedprice:</b> Preço esperado do veículo

- <b>mileage:</b> - Quilometragem rodada

- <b>condition:</b> Estado de conservação do veículo

<hr>

<h2>Estrutura do Relatório</h2>

O projeto será estruturado em duas páginas, sendo a primeira apenas como uma imagem introdutória para ir para a página secundária que é onde está o Dashboard contendo os gráficos e informações.

<hr>

<h1>Visão Geral de Vendas</h1>

- <b>Objetivo:</b> Apresenta um panorama geral das vendas no geral.

- Gráficos e Elementos contidos na página:

<b>Cartão</b>

Exibe o Total de Vendas.

Medida DAX contida no gráfico:

```python
Total Vendas = SUM('car_prices'[sellingprice])
```

<hr>

<b>Cartão</b>

Exibe a marca de carro mais vendida.

Medida DAX contida no gráfico:

```python
Fabricante Mais Vendida = 
VAR FabricanteTop = 
    TOPN(1, 
        SUMMARIZE('car_prices', 'car_prices'[make], "@TotalVendas", COUNT('car_prices'[make])), 
        [@TotalVendas], 
        DESC
    )
RETURN 
    SELECTCOLUMNS(FabricanteTop, "Fabricante", 'car_prices'[make])
```

<hr>

<b>Cartão</b>

Exibe a cor de carro mais vendida.

Medida DAX contida no gráfico:

```python
Cor Mais Vendida = 
VAR CorTop = 
    TOPN(1, 
        SUMMARIZE('car_prices', 'car_prices'[color], "@TotalVendas", COUNT('car_prices'[color])), 
        [@TotalVendas], 
        DESC
    )
RETURN 
    SELECTCOLUMNS(CorTop, "Cor", 'car_prices'[color])
```

<hr>

<b>Cartão</b>

Exibe o modelo do carro mais vendido.

Medidas DAX contidas no gráfico:

```python
Modelo Mais Vendido = 
VAR ModeloTop = 
    TOPN(1, 
        SUMMARIZE('car_prices', 'car_prices'[model], "@TotalVendas", COUNT('car_prices'[model])), 
        [@TotalVendas], 
        DESC
    )
RETURN 
    SELECTCOLUMNS(ModeloTop, "Modelo", 'car_prices'[model])
```

<hr>

<b>Gráfico de Colunas Empilhadas e Linha</b>

Exibe as vendas totais e veículos vendidos ao longo do tempo.

Medida DAX contida no gráfico:

```python
Quantidade Veículos Vendidos = COUNT('car_prices'[vin])
```

```python
Total Vendas = SUM('car_prices'[sellingprice])
```

Eixo X: Year(Ano)

Eixo Y da Coluna: Total Vendas

Eixo Y da Linha: Quantidade Veículos Vendidos

<hr>

<b>Gráfico de Dispersão</b>

Exibe a previsão do preço do carro comparado ao preço atual do carro.

Medida DAX contida no gráfico:

```python
Previsão Preço = 
    VAR media_mmr = AVERAGE('car_prices'[mmr])
    VAR media_odometer = AVERAGE('car_prices'[odometer])
    VAR fator_condicao = AVERAGE('car_prices'[condition]) * 500
    RETURN media_mmr - (media_odometer * 0.05) + fator_condicao
```

Eixo X: sellingprice

Eixo Y: Previsão Preço

<hr>

<b>Gráfico de Colunas Clusterizado</b>

Exibe a média de preço por ano.

Eixo X: year(Ano)

Eixo Y: Média de sellingprice

<hr>

<b>Mapa</b>

Exibe o total de vendas por região.

Localização: state

Tamanho da bolha: Total Vendas

<h1>Considerações Técnicas</h1>

<b>Preparação dos Dados:</b>

Foram realizadas transformações para corrigir tipos de dados, tratamento de valores nulos, extração de caracteres, transformação de caracteres, tratamento de vazios.

<hr>
