# Fator casa no Brasileirão — Projeto Final de Análise de Dados

Análise das 8.785 partidas da Série A do Campeonato Brasileiro de 2003 a 2024. O projeto passa por seleção, diagnóstico, tratamento, análise exploratória e visualização dos dados, e termina num dashboard no Looker Studio.

**Pergunta:** a vantagem de jogar em casa existe, está mudando, e o que mais explica gols e vitórias?

## Principais resultados
- Em casa os clubes conquistam **58,4%** dos pontos; fora, **32,8%**.
- A vitória do mandante caiu de **52,0%** (2003–2009) para **46,1%** (2021–2024). Em 2020, temporada sem público, foi **45,0%**.
- Chutes no alvo são a estatística mais ligada a gols (Spearman **+0,48**). A correlação da posse de bola com gols é praticamente nula (**−0,06**).
- **55,7%** dos gols saem no 2º tempo. A fatia de gols nos acréscimos subiu de 5,1% (2014) para 8–9% (2020–2024).

## Estrutura
| Caminho | Conteúdo |
|---|---|
| `projeto_final_brasileirao.ipynb` | Notebook completo, já executado |
| `dados_brutos/` | 4 CSVs originais (partidas, estatísticas, gols, cartões) |
| `dados_tratados/` | 6 CSVs limpos para o Looker Studio |
| `relatorio_insights.pdf` | Relatório com os principais insights |
| `GUIA_LOOKER_STUDIO.md` | Passo a passo do dashboard |

## Como rodar
```bash
pip install pandas numpy matplotlib jupyter
jupyter notebook projeto_final_brasileirao.ipynb
```
Se a pasta `dados_brutos/` não existir, o notebook baixa os arquivos direto da fonte.

**Fonte dos dados:** [Brasileirão Dataset — adaoduque](https://github.com/adaoduque/Brasileirao_Dataset) (dados públicos).

**Dashboard (Looker/Data Studio):** https://datastudio.google.com/reporting/1bb9fca4-57a3-4597-bbc5-22fd8b271d8b

**Autor:** Thiago Paiano — [LinkedIn](https://linkedin.com/in/thiago-paiano) · [GitHub](https://github.com/Paianou)
