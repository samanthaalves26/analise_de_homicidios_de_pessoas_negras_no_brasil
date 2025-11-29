<div style="font-family: Arial, sans-serif; line-height: 1.6;">

<h1 style="text-align:center; font-size: 32px; margin-bottom: 10px;">
📊 <strong>Análise de Homicídios de Pessoas Negras no Brasil — 2022</strong>
</h1>

<p style="text-align:center; font-size: 18px; color: #444;">
Estudo exploratório e inferencial relacionando <strong>PIB per capita</strong> e a <strong>taxa de homicídios da população negra</strong> nos estados brasileiros.
</p>

<hr style="margin: 30px 0;">

<h2>🧭 Objetivo Geral</h2>

<p>
O projeto investiga se existe relação entre o nível econômico dos estados (PIB per capita — IBGE, 2022) e a letalidade violenta contra pessoas negras (taxa por 100 mil habitantes negros).
</p>

<ul>
  <li>Calcular a <strong>taxa de homicídios da população negra por UF</strong>;</li>
  <li>Construir base integrada com dados do <strong>Censo 2022</strong> (população total e negra);</li>
  <li>Processar dados de <strong>PIB por UF</strong>;</li>
  <li>Criar visualizações estatísticas e comparativas;</li>
  <li>Testar a hipótese: <em>PIB per capita tem relação negativa com a taxa de homicídios da população negra</em>.</li>
</ul>

<hr style="margin: 30px 0;">

<h2>📚 Fontes de Dados</h2>

<h3>🔸 Atlas da Violência — IPEA / FBSP</h3>
<p>Número de homicídios por raça/cor por UF.</p>
<p><strong>Arquivo:</strong> <code>homicidios-negros.csv</code></p>

<h3>🔸 Censo 2022 — IBGE</h3>
<p>População total e população autodeclarada preta/parda (consolidada como <code>pop_negra</code>).</p>
<p><strong>Arquivo gerado:</strong> <code>censo_2022_pop_negra.csv</code></p>

<h3>🔸 PIB por Estado — IBGE</h3>
<p>PIB dos estados em milhares de reais.</p>
<p><strong>Arquivo:</strong> <code>pib_estados_2022.csv</code></p>

<hr style="margin: 30px 0;">

<h2>⚙️ Ambiente e Dependências</h2>

<div style="background:#111; padding:15px; border-radius:8px; color:#eee; margin: 10px 0;">
<pre>
python -m venv .venv
source .venv/bin/activate     # Linux / macOS
.venv\Scripts\activate        # Windows

pip install -r requirements.txt
</pre>
</div>

<p><strong>requirements.txt:</strong></p>

<div style="background:#111; padding:15px; border-radius:8px; color:#eee;">
<pre>
pandas
numpy
matplotlib
seaborn
scipy
statsmodels
unidecode
IPython
</pre>
</div>

<hr style="margin: 30px 0;">

<h2>🧹 Limpeza e Preparação dos Dados</h2>

<ul>
  <li>Leitura e padronização de nomes de estados;</li>
  <li>Filtragem do ano 2022;</li>
  <li>Remoção de entradas nacionais (ex.: "Brasil");</li>
  <li>Conversão de tipos numéricos;</li>
  <li>Merge entre bases (Censo × Homicídios × PIB);</li>
  <li>Cálculo de PIB per capita;</li>
  <li>Cálculo da taxa de homicídios da população negra;</li>
  <li>Verificação de valores nulos/zeros.</li>
</ul>

<div style="background:#111; padding:15px; border-radius:8px; color:#eee;">
<pre>
df_h = pd.read_csv("homicidios-negros.csv", sep=";")
df_2022 = df_h[df_h["período"] == 2022]

df_merged = df_2022.merge(df_censo, on="nome")
df_merged["taxa_homicidios_negros"] = (
    df_merged["valor"] / df_merged["pop_negra"]
) * 100000

df_merged["pib_per_capita"] = df_merged["pib_reais"] / df_merged["pop_total"]
</pre>
</div>

<hr style="margin: 30px 0;">

<h2>📈 Visualizações e Estatísticas</h2>

<ul>
  <li>Barras com população total e negra (Top 10);</li>
  <li>Barras com número absoluto de homicídios por UF;</li>
  <li>Barras com PIB per capita (Top 10);</li>
  <li>Barras com maiores taxas de homicídio da população negra;</li>
  <li>Scatterplot PIB per capita × taxa de homicídios, com linha de regressão;</li>
  <li>Cálculo de Pearson e regressão linear (opcional).</li>
</ul>

<div style="background:#111; padding:15px; border-radius:8px; color:#eee;">
<pre>
from scipy.stats import pearsonr

r, p = pearsonr(
    df_merged["pib_per_capita"],
    df_merged["taxa_homicidios_negros"]
)

print(f"Pearson r = {r:.2f}, p = {p:.4f}")
</pre>
</div>

<hr style="margin: 30px 0;">

<h2>📌 Principais Resultados</h2>

<h3>1. Panorama Absoluto</h3>
<p>
Estados mais populosos — SP, MG, RJ — apresentam maior número absoluto de homicídios, como esperado pelo tamanho populacional.
</p>

<h3>2. Taxa por 100 mil (População Negra)</h3>
<p>
Norte e Nordeste concentram as maiores taxas, com destaque para <strong>Amapá, Alagoas, Pernambuco e Amazonas</strong>.
</p>

<h3>3. Correlação estatística</h3>

<div style="background:#111; padding:15px; border-radius:8px; color:#eee;">
<pre>
r ≈ -0.62   # correlação negativa moderada
p &lt; 0.05    # estatisticamente significativo
</pre>
</div>

<p>
Interpretando: estados com maior <strong>PIB per capita</strong> tendem a apresentar <strong>menores taxas de homicídios da população negra</strong>.
</p>

<h3>4. Padrões Regionais</h3>
<p>
A combinação entre desigualdade histórica, concentração populacional negra e vulnerabilidade territorial reforça um padrão estrutural presente principalmente no Nordeste e na Amazônia Legal.
</p>

<hr style="margin: 30px 0;">

<h2>📦 Estrutura Geral do Projeto</h2>

<ul>
  <li>🗂 CSVs originais + pré-processados</li>
  <li>📓 Jupyter Notebook / Scripts Python</li>
  <li>📊 PNGs com visualizações (dpi 300)</li>
  <li>📁 README interativo (este)</li>
</ul>

</div>
