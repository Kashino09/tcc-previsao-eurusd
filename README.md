# 📈 Previsão do EUR/USD com Machine Learning e Incerteza das Previsões

## 📑 Índice
- Contexto
- Objetivos
- Fontes
- Metodologia
- Resultados
- Incerteza das Previsões
- Acompanhamento Prospectivo
- Limitações e Considerações Éticas
- Trabalhos Futuros
- Links
- Autor

# Contexto:
- Este projeto é o Trabalho de Conclusão de Curso (TCC) do MBA em Inteligência Artificial e Big Data do ICMC/USP. O estudo compara modelos de previsão da taxa de câmbio EUR/USD em horizontes curtos e mede, além da previsão pontual, a incerteza associada a ela.
- O trabalho foi desenvolvido em Python, em notebooks do Kaggle.

# Objetivos:
- Comparar o desempenho de modelos estatísticos e de aprendizado de máquina na previsão do retorno do EUR/USD, com o mesmo protocolo para os quatro;
- Avaliar dois horizontes de previsão: **t+1** (1 dia) e **t+5** (1 semana útil);
- Incluir uma variável binária de eventos geopolíticos (9 eventos, com janela de 5 dias corridos para cada lado) e verificar se ela ajuda na previsão;
- Construir intervalos de previsão com **Adaptive Conformal Inference (ACI)** e medir a cobertura obtida;
- Validar os modelos fora da amostra e acompanhar o desempenho de forma prospectiva, com dados reais.

# Fontes:
- Série diária do EUR/USD de janeiro de 2010 a abril de 2025 (3.990 observações), em retorno logarítmico, obtida por meio da biblioteca `yfinance`;
- Variáveis de eventos geopolíticos construídas para o estudo (janelas de evento em torno das datas selecionadas);
- Referências bibliográficas do TCC (20 no total).

# Metodologia
**Modelos comparados**
- **ARIMA**, como modelo estatístico clássico de séries temporais;
- **Elastic Net**, regressão regularizada, com padronização das variáveis e busca de `alpha` e `l1_ratio`;
- **Random Forest**;
- **LSTM**, rede neural recorrente.

**Etapas**
1. Preparação dos dados e construção das variáveis, incluindo as janelas de eventos geopolíticos;
2. Divisão em treino e teste (80/20);
3. Treinamento e avaliação dos quatro modelos nos horizontes t+1 e t+5;
4. Construção dos intervalos com ACI e cálculo da cobertura;
5. Congelamento dos modelos treinados e validação fora da amostra (de maio de 2025 até a data da análise), para ARIMA, Elastic Net e Random Forest;
6. Acompanhamento prospectivo diário, a partir de 1º de setembro de 2026, comparando os dados reais com os intervalos previstos.

# Resultados
Métricas no conjunto de teste (menor é melhor):

| Horizonte | Modelo | RMSE | MAE |
|---|---|---|---|
| t+1 | ARIMA | 0,005116 | 0,003816 |
| t+1 | Elastic Net | 0,005121 | 0,003820 |
| t+1 | Random Forest | 0,005154 | 0,003889 |
| t+1 | LSTM | 0,005334 | 0,004009 |
| t+5 | ARIMA | 0,011200 | 0,008303 |
| t+5 | Elastic Net | 0,011288 | 0,008371 |
| t+5 | Random Forest | 0,012708 | 0,009877 |
| t+5 | LSTM | 0,011826 | 0,008812 |

**Principais achados**
- Os erros dos modelos ficaram muito próximos, e **nenhum modelo superou o benchmark de não-variação**, representado pelo ARIMA;
- A ACF e a PACF dos retornos não mostram autocorrelação relevante;
- Os coeficientes do Elastic Net convergiram para valores próximos de zero nos dois horizontes, o que reforça a hipótese de **eficiência de mercado** para o câmbio;
- O MAPE se mostrou instável nessa série, por isso a análise prioriza RMSE e MAE.

# Incerteza das Previsões
- Nos dados históricos, a cobertura dos intervalos ficou entre **89,3% e 89,7%** nos quatro modelos e dois horizontes (meta: 90%);
- Na validação fora da amostra, a cobertura foi de **91,8%** para ARIMA, Elastic Net e Random Forest;
- Na validação fora da amostra, os erros foram menores que no teste original, e o MAPE do Random Forest ficou bem mais próximo dos demais.

# Acompanhamento Prospectivo
- De 1º a 23 de setembro de 2026, os modelos congelados (ARIMA, Elastic Net e Random Forest) fizeram previsões diárias, registradas antes de o dado real ser conhecido;
- Com **16 observações confirmadas**, a cobertura do intervalo ACI foi de **75,0%**, e o acerto direcional foi de **68,8%** (Elastic Net) e **62,5%** (Random Forest);
- A amostra é muito pequena: uma única observação desloca a cobertura em mais de 6 pontos percentuais, então esses números devem ser lidos com cautela e não indicam capacidade preditiva;
- O estado do acompanhamento é mantido em um Dataset do Kaggle, atualizado manualmente.

# Limitações e Considerações Éticas
- Este trabalho tem finalidade acadêmica e **não é recomendação de investimento**;
- Os resultados indicam que prever o câmbio de curto prazo é difícil, e os modelos não devem ser usados para decisões de negociação;
- Limitações: um único par de moedas (EUR/USD), apenas 16 observações prospectivas, LSTM fora da validação fora da amostra e do acompanhamento prospectivo, e variável de evento geopolítico binária;
- A comunicação da incerteza, por meio dos intervalos, é parte central do estudo, e não um detalhe.

# Trabalhos Futuros
- R² fora da amostra (R²OS) como métrica complementar e acompanhamento prospectivo mais longo;
- Outros pares de moedas e estratégias de carry trade;
- Variantes de Conformal Prediction, como CQR e Mondrian;
- Camada adaptativa de largura de intervalo, em função da volatilidade e da divergência entre modelos;
- Eventos macroeconômicos e notícias estruturadas, com controle rígido de carimbo de tempo para evitar vazamento de informação futura;
- Combinação adaptativa de modelos por regime de mercado;
- Opção de abstenção, em que o modelo não emite previsão quando a incerteza é alta.

# Estrutura do Repositório
| Arquivo | Descrição |
|---|---|
| `tcc-cambio-usdeur.ipynb` | Notebook completo, da coleta dos dados ao acompanhamento prospectivo |
| `requirements.txt` | Bibliotecas usadas no projeto |
| `Apresentação MBA.pdf` | Slides da apresentação do trabalho |
| `01` a `11` (arquivos `.png`) | Figuras geradas pelo notebook |
| `LICENSE` | Licença MIT |

# Figuras
**Comparação das métricas (t+1 e t+5)**

![Comparação de métricas entre os modelos](06%20-%20Compara%C3%A7%C3%A3o%20M%C3%A9tricas.png)

**Intervalos ACI em t+1 (últimos 120 dias)**

![Intervalos ACI em t+1](07%20-%20ACI%20Intervalo%20t%2B1.png)

**Validação fora da amostra (mai/2025 a set/2026)**

![Validação fora da amostra](10%20-%20Valida%C3%A7%C3%A3o%20OOT.png)

# Como Reproduzir
1. Instale as dependências: `pip install -r requirements.txt`;
2. Abra o notebook `tcc-cambio-usdeur.ipynb`. Ele foi escrito para rodar no Kaggle, então os caminhos `/kaggle/working` e `/kaggle/input` precisam ser ajustados para rodar em outro ambiente;
3. Execute as células na ordem. Os dados do EUR/USD são baixados pelo `yfinance`, e por isso os dados brutos não estão neste repositório;
4. Observações: a validação fora da amostra baixa os dados até a data de execução, então seus resultados mudam com o tempo; a rotina de acompanhamento prospectivo (seção 9.8) depende do estado salvo do dia anterior; e o treinamento do LSTM pode gerar números ligeiramente diferentes entre execuções, mesmo com sementes fixas.

# Links
- Notebook neste repositório: [`tcc-cambio-usdeur.ipynb`](tcc-cambio-usdeur.ipynb)
- Notebook no Kaggle: [TCC Câmbio USD/EUR](https://www.kaggle.com/code/kelwinpaschoal/tcc-cambio-usdeur)
- Dataset do acompanhamento prospectivo: [tcc-cambio-estado no Kaggle](https://www.kaggle.com/datasets/kelwinpaschoal/tcc-cambio-estado)

# Autor
- Kelwin Paschoal
