📊 ANÁLISE DE HOMICIDIOS DE PESSOAS NEGRAS NO BRASIL
Análise exploratória e inferencial sobre a relação entre PIB per capita e a taxa de homicídios de pessoas negras no Brasil (2022).

Projeto desenvolvido em Python (Jupyter/Notebook/Scripts) com geração de gráficos PNG e tabelas CSV.

🧭 Objetivo do projeto

Investigar se existe uma relação entre o nível econômico de cada estado (medido pelo PIB per capita em 2022) e a letalidade violenta sobre a população negra (homicídios por 100.000 habitantes negros). Especificamente:

Calcular a taxa de homicídios de pessoas negras por UF (óbitos por 100.000 habitantes negros).

Obter e preparar dados de população total, população negra e PIB 2022.

Visualizar padrões absolutos (nº homicídios, população) e relativos (taxas).

Testar a hipótese: PIB per capita tem relação negativa com a taxa de homicídios da população negra (usar Pearson r e avaliar significância).

📚 Fontes de dados (origem)

As fontes utilizadas e como cada conjunto foi empregado:

Atlas da Violência / IPEA / FBSP (dados de homicídios por raça/cor)

Contém nº de homicídios por raça/cor por Unidade Federativa.

Arquivo no repositório: homicidios-negros.csv (separador ;).

Usado para extrair os homicídios de pessoas negras em 2022 (período == 2022).

Censo 2022 (IBGE)

População total por UF e população que se autodeclarou preta/ parda/negra (aqui consolidada como pop_negra).

No repositório: geramos censo_2022_pop_negra.csv (exemplo de construção manual contido no notebook/script).

Usado como denominador para calcular taxa por 100.000.

PIB dos estados (IBGE / tabela consolidada)

PIB por UF (aqui fornecido em milhares de reais no arquivo pib_estados_2022.csv gerado no script).

Usado para calcular pib_per_capita (PIB em reais / população total).

Dependências / Ambiente

Recomendado criar um ambiente virtual (venv / conda). Instale com:

python -m venv .venv
source .venv/bin/activate   # Linux / macOS
.venv\Scripts\activate      # Windows

pip install -r requirements.txt

Exemplo requirements.txt (contém bibliotecas usadas no notebook/script):
pandas
numpy
matplotlib
seaborn
scipy
statsmodels
unidecode
IPython

Limpeza e preparação dos dados (detalhes técnicos)

As principais etapas de pré-processamento aplicadas:

Leitura e padronização

df_homicidios_negros = pd.read_csv("homicidios-negros.csv", sep=";")

Normalizar nomes de UF (unidecode / upper) se necessário.

Filtrar o ano de interesse

df_2022 = df_homicidios_negros[df_homicidios_negros["período"] == 2022].

Remover agregados nacionais

Garantir que o dataset contenha apenas UFs (remover linha "Brasil" / "BR" se existir) para evitar distorções.

Tipos numéricos e conversões

Converter colunas numéricas (valor, pop_total, pop_negra, pib_mil_reais) para int/float.

Converter PIB: pib_reais = pib_mil_reais * 1000.

Merge das tabelas

df = df_pib.merge(df_censo, on="nome", how="inner") — ou df_homicidios com df_censo para taxas.

Cálculo do PIB per capita

df["pib_per_capita"] = df["pib_reais"] / df["pop_total"].

Cálculo da taxa (homicídios por 100k habitantes negros)

taxa = (n_homicidios_negros / pop_negra) * 100000.

Tratamento de zeros / NaNs

Verificar pop_negra == 0 (evitar divisão por zero), preencher ou excluir conforme justificativa.

Código — o que cada parte faz (resumo técnico)

No notebook/script principal foram implementadas as seguintes rotinas:

Construção / leitura de DataFrames: df_censo, df_pib, df_homicidios_negros.

Visualizações com Matplotlib / Seaborn:

Barras: população total (Top10), população negra (Top10), número absoluto de homicídios por UF.

Barras: PIB per capita (Top10).

Barras: Top10 taxa de homicídios de negros (por 100k).

Scatter com linha de regressão (seaborn.regplot) para pib_per_capita x taxa_homicidios_negros, com rótulos dos pontos (nomes das UFs).

Estatística:

Cálculo do coeficiente de correlação de Pearson: pearsonr(x, y) (resulta em r e p-value).

Regressão linear simples via statsmodels (opcional) para estimar inclinação, intercepto e p-values dos coeficientes.

Export:

Salvar CSVs processados e PNGs de cada figura (dpi=300, bbox_inches="tight").

Trecho essencial (exemplo):
# merge e cálculo de taxa
df_merged = df_homicidios_ultimo.merge(df_censo, on="nome")
df_merged["taxa_homicidios_negros"] = (df_merged["valor"] / df_merged["pop_negra"]) * 100000

# correlação
r, p = pearsonr(df_merged["pib_per_capita"], df_merged["taxa_homicidios_negros"])
print(f"Pearson r = {r:.2f}, p = {p:.4f}")

Principais resultados (interpretação)

Panorama absoluto: Estados com maior população total (SP, MG, RJ) concentram maior número absoluto de homicídios — porém esse resultado é esperado por efeito de escala populacional.

Taxa por 100k (população negra): Estados do Norte e Nordeste aparecem com as maiores taxas (ex.: Amapá, Alagoas, Pernambuco, Amazonas), indicando risco desproporcional mesmo quando ajustado pela população negra.

Correlação:

Coeficiente de Pearson estimado: r ≈ -0.62 (conforme o artigo).

Interpretação: correlação negativa moderada — em média, maiores PIB per capita associam-se a menores taxas de homicídio entre pessoas negras.

O p-value foi reportado como estatisticamente significativo (p < 0.05), o que fortalece a hipótese de associação (não prova causalidade).

Mapeamento espacial e distinções regionais:

Concentração de população negra e altas taxas no Nordeste e parte da Amazônia Legal aponta para um padrão territorial de vulnerabilidade.
