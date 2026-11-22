# Guia — Dashboard do Brasileirão no Looker Studio

Tempo estimado: 1h30. Tudo usa os arquivos da pasta `dados_tratados/`.

## 1. Subir os dados

1. Acesse https://lookerstudio.google.com e clique em **Criar → Relatório**.
2. Em "Adicionar dados", escolha o conector **Upload de arquivo** e envie `jogos_clube.csv`. Clique em **Adicionar**.
3. Repita para `partidas.csv`, `classificacao.csv`, `gols.csv`, `cartoes.csv` e `estatisticas.csv`
   (menu **Recurso → Gerenciar fontes de dados adicionadas → Adicionar uma fonte de dados**).
4. Em cada fonte, confira os tipos de campo (**Recurso → Gerenciar fontes → Editar**):
   - `data` → **Data (AAAA-MM-DD)**
   - `temporada` → **Número** (ou Texto, se preferir que apareça sem separador de milhar: "2024" em vez de "2.024")
   - `mandante_estado_iso`, `visitante_estado_iso`, `estado_iso` → **Geográfico → Código de subdivisão do país**
   - `posse_de_bola`, `precisao_passes` → Número

> Alternativa: suba os CSVs para o Google Sheets e use o conector Planilhas Google. Funciona igual e permite atualizar depois.

## 2. Campos calculados

Crie em cada fonte (**Editar fonte → Adicionar um campo**):

**Na fonte `jogos_clube`**
| Nome | Fórmula |
|---|---|
| Aproveitamento | `SUM(pontos) / (COUNT(partida_id) * 3)` → formato Percentual |
| Vitórias | `SUM(CASE WHEN resultado_clube = "Vitória" THEN 1 ELSE 0 END)` |

**Na fonte `partidas`**
| Nome | Fórmula |
|---|---|
| % Vitória mandante | `SUM(CASE WHEN resultado = "Vitória mandante" THEN 1 ELSE 0 END) / COUNT(partida_id)` |
| % Empate | `SUM(CASE WHEN resultado = "Empate" THEN 1 ELSE 0 END) / COUNT(partida_id)` |
| % Vitória visitante | `SUM(CASE WHEN resultado = "Vitória visitante" THEN 1 ELSE 0 END) / COUNT(partida_id)` |
| Média de gols | `AVG(total_gols)` |

**Na fonte `estatisticas`**
| Nome | Fórmula |
|---|---|
| % Vitória | `SUM(CASE WHEN resultado_clube = "Vitória" THEN 1 ELSE 0 END) / COUNT(partida_id)` |

## 3. Página 1 — Fator casa

| Elemento | Tipo | Configuração |
|---|---|---|
| Filtros (no topo) | Controle de intervalo / Lista suspensa | `temporada` (controle deslizante), `clube`, `mando` — fonte `jogos_clube` |
| 4 indicadores | Visão geral (scorecard) | Fonte `partidas`: Contagem de `partida_id`, Média de gols, % Vitória mandante, % Empate |
| Resultados por temporada | Gráfico de linhas | Fonte `partidas` · Dimensão `temporada` · Métricas: % Vitória mandante, % Empate, % Vitória visitante |
| Casa × fora por clube | Gráfico de barras | Fonte `jogos_clube` · Dimensão `clube` · Detalhamento `mando` · Métrica Aproveitamento · ordenar por Aproveitamento |
| Mapa | Mapa geográfico (Google Maps / geográfico) | Fonte `jogos_clube` · Dimensão `estado_iso` · Métrica Aproveitamento · Filtro: `mando` = Casa |

Para os filtros valerem para gráficos de fontes diferentes, use em todas as fontes os mesmos nomes de campo (`temporada` já é igual) ou agrupe os gráficos e o controle (**Organizar → Agrupar**).

## 4. Página 2 — Campeões

| Elemento | Tipo | Configuração |
|---|---|---|
| Classificação | Tabela dinâmica | Fonte `classificacao` · Linhas `temporada` · Colunas `posicao` (filtro `posicao` ≤ 4) · Métrica `pontos` |
| Tabela de classificação | Tabela | `posicao`, `clube`, `pontos`, `vitorias`, `saldo`, `aproveitamento` · controle de filtro por `temporada` |
| Títulos por clube | Gráfico de barras | Filtro `posicao` = 1 · Dimensão `clube` · Métrica Contagem |

## 5. Página 3 — Dentro do jogo

| Elemento | Tipo | Configuração |
|---|---|---|
| Gols por minuto | Gráfico de colunas | Fonte `gols` · Dimensão `faixa_minuto` · Métrica Contagem · ordenar a dimensão em ordem crescente |
| Cartões por posição | Barras empilhadas | Fonte `cartoes` · Dimensão `posicao` · Detalhamento `cartao` |
| Chutes no alvo × gols | Dispersão | Fonte `estatisticas` · Dimensão `clube` (e `temporada`) · X = Média de `chutes_no_alvo` · Y = Média de `gols_pro` |
| Posse × vitória | Gráfico de colunas | Fonte `estatisticas` · Dimensão `faixa_posse` · Métrica % Vitória |

## 6. Acabamento e publicação

- Título em cada página e uma frase com o insight principal (ex.: "Mandante venceu 52,0% em 2003–2009 e 46,1% em 2021–2024").
- Use no máximo 2 ou 3 cores: azul para "Casa", laranja para "Fora", cinza para o resto.
- **Compartilhar → Gerenciar acesso → Qualquer pessoa com o link pode ver** e cole o link no relatório (seção "Dashboard interativo").
