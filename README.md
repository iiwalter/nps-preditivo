# Tech Challenge Fase 1 — Case NPS (e-commerce)

Repositório do case **NPS preditivo** (Fase 1), com análise em notebook e dados operacionais de e-commerce.

---

## Objetivo do projeto

Explorar a base histórica de pedidos, logística e atendimento para **entender padrões associados ao NPS** (`nps_score`) e à experiência do cliente, com **foco em negócio** (perguntas do tipo: fatores mais críticos, detratores, possíveis “pontos de ruptura” na jornada, perfis de cliente).

O material complementar de **entendimento de negócio**, **definição da variável-alvo** e **apresentação em slides/vídeo** pode estar no próprio notebook, neste README ou nos arquivos de apresentação, conforme organização da equipe.

O **modelo preditivo** descrito no desafio é **opcional**; a prioridade deste repositório é a **EDA** e a reprodutibilidade do fluxo no `nps_preditivo.ipynb`.

---

## Descrição da base de dados

| Item | Detalhe |
|------|---------|
| **Arquivo** | `desafio_nps_fase_1.csv` (fornecido no material do Tech Challenge) |
| **Conteúdo** | Registros de pedidos com variáveis de cliente, pedido, entrega, atendimento e indicadores de satisfação |

**Variáveis principais**

| Grupo | Campos |
|-------|--------|
| Identificação | `customer_id`, `order_id` |
| Cliente | `customer_age`, `customer_region`, `customer_tenure_months` |
| Pedido | `order_value`, `items_quantity`, `discount_value`, `payment_installments` |
| Logística | `delivery_time_days`, `delivery_delay_days`, `freight_value`, `delivery_attempts` |
| Atendimento / pós-venda | `customer_service_contacts`, `resolution_time_days`, `complaints_count` |
| Comportamento / internos | `repeat_purchase_30d` (0/1 — recompra em até 30 dias), `csat_internal_score` |
| **Satisfação (alvo da análise)** | `nps_score` — nota de NPS após a experiência de compra (escala 0 a 10 no material do case) |
| **Variável derivada (criada no notebook)** | `nps_class` — classificação em **Detratores**, **Passivos** ou **Promotores** com base em `nps_score`, segundo os limiares definidos no código; **não** existe no CSV original — é gerada na preparação dos dados para contagens, recortes e gráficos por categoria. |

---

## Metodologia utilizada

1. **Carregamento e preparação:** leitura do CSV com `pandas`; checagem de dimensão, tipos (`dtypes`), nomes das colunas e **valores ausentes**.
2. **Distribuição do NPS:** estatísticas descritivas e visualizações (histogramas, dispersão) para situar a variável-alvo.
3. **Classificação NPS:** função ou regra sobre `nps_score` para criar a coluna **`nps_class`** (Detratores / Passivos / Promotores), usada em contagens e recortes.
4. **Análise por região:** médias de variáveis de entrega e NPS por `customer_region`; distribuição de categorias NPS por região.
5. **Associação com o NPS:** matriz de **correlação** (Pearson) entre fatores operacionais e `nps_score`; **mapa de calor** para leitura rápida dos sinais mais fortes.
6. **Detratores vs. base:** médias de fatores operacionais entre clientes classificados como detratores e na base geral; gráficos comparativos.
7. **Recompra:** subconjunto sem recompra em 30 dias (`repeat_purchase_30d == 0`) para correlações e leitura de “ponto de ruptura”; comparações adicionais entre grupos com/sem recompra conforme o notebook.
8. **Inferência (se presente no notebook):** estimativa da média de NPS com **intervalo de confiança** (e/ou bootstrap), com nível de confiança e lógica de amostragem descritos nas células correspondentes — o IC reflete **incerteza estatística** da estimativa; **viés de não resposta** à pesquisa, quando discutido, é tratado como limitação conceitual separada.

**Ferramentas:** Python 3, `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly` (conforme imports do notebook).

**Limitações:** correlações e médias indicam **associação**, não causalidade; conclusões valem para o **escopo temporal e amostral** do CSV do case.

---

## Como reproduzir os resultados
---

### No Google Colab

1. Abrir o notebook: enviar o arquivo `nps_preditivo.ipynb` (*File → Upload notebook*) ou usar o botão **“Open in Colab”** no proprio arquivo.
2. Subir o CSV: no menu lateral (**ícone de pasta** / *Files*) → *Upload* → enviar **`desafio_nps_fase_1.csv`** para o ambiente (cole na raiz da aba Files/Arquivos).
3. *Runtime* → **Run all** (ou ir célula a célula com Shift+Enter).

---

### No computador (ambiente local)

1. Instalar **Python 3.10+** (ou a versão indicada pela disciplina).
2. (Opcional) Criar ambiente virtual e ativá-lo.
3. Instalar dependências:

   ```bash
   pip install pandas numpy seaborn matplotlib plotly jupyter
   ```

4. **Baixar o repositório** (clone Git ou ZIP) e colocar **`desafio_nps_fase_1.csv`** na pasta que o notebook espera (ex.: mesma pasta do `.ipynb` ou `data/`).
5. Ajustar o caminho em `pd.read_csv(...)` se necessário, por exemplo:

   ```python
   nps_data = pd.read_csv("../data/desafio_nps_fase_1.csv")
   # ou manter: pd.read_csv("desafio_nps_fase_1.csv")
   ```

6. Abrir o notebook no **Jupyter Lab**, **Jupyter Notebook** ou **VS Code** e executar as células **em ordem** (*Run All*).

---
**Estrutura de pastas**

```
.
├── README.md
├── data/
│   └── desafio_nps_fase_1.csv
└── notebooks/
    └── eda-nps.ipynb
```

---

## Autoria

- Amanda Rarymi Affanio
- Samuel Porto Alcala dos Santos
- Walter Henrique Costa Ferreira
