# reducao-custos-farmacias-aws
Proposta arquitetural e relatório técnico de redução de custos operacionais e modernização de infraestrutura em nuvem para rede farmacêutica utilizando AWS.

# Redução dos Custos em Farmácias com AWS

Este projeto apresenta um relatório técnico e arquitetural focado na migração e otimização de custos de infraestrutura de TI para uma rede de farmácias com unidades físicas e operação de e-commerce, aplicando as melhores práticas do pilar de Otimização de Custos do **AWS Well-Architected Framework**.

---

## 1. Contexto do Desafio

A rede de farmácias operava com servidores locais (*on-premises*) dimensionados para acomodar picos sazonais de vendas, resultando em:
* Alta ociosidade computacional durante períodos de baixo movimento e madrugadas.
* Custos elevados com licenciamento, manutenção física, climatização e nobreaks.
* Volume massivo de retenção de receitas médicas digitalizadas, notas fiscais e relatórios regulatórios (ANVISA), onerando sistemas de armazenamento locais caros.
* Ausência de visibilidade granular sobre o custo operacional de TI por filial.

---

## 2. Objetivos Principais

1. **Redução do TCO (*Total Cost of Ownership*):** Reduzir em pelo menos 35% os gastos gerais com infraestrutura e manutenção.
2. **Elasticidade Computacional:** Adequar a capacidade de servidores ao volume real de requisições de caixa (PDV) e delivery.
3. **Armazenamento em Camadas com Conformidade:** Reter prescrições e logs regulatórios com segurança pelo período exigido por lei com custo mínimo de repouso.
4. **Governança Financeira:** Estabelecer controle contínuo de orçamento e alertas preditivos de despesas em nuvem.

---

## 3. Arquitetura e Serviços AWS Propostos

### Computação e Aplicação
* **Amazon EC2 com Auto Scaling Groups:** Ajuste automático do número de instâncias web e de API conforme a demanda horária de pedidos, garantindo performance em horários de pico e desligamento de capacidade ociosa à noite.
* **AWS Compute Savings Plans:** Contratação de planos de compromisso de 1 a 3 anos para a carga de trabalho de linha de base (*baseline*), proporcionando até 60% de economia frente ao modelo *On-Demand*.

### Armazenamento de Arquivos e Receitas Digitais
* **Amazon S3 (Simple Storage Service):** Repositório central de imagens de receitas, exames e documentos fiscais.
* **S3 Intelligent-Tiering & S3 Glacier:** Implementação de políticas de ciclo de vida (*Lifecycle Policies*):
  * **Primeiros 30 dias:** Armazenamento em *S3 Standard* ou *Intelligent-Tiering* para acesso imediato das farmácias.
  * **Após 90 dias:** Transição para *S3 Glacier Flexible Retrieval*, reduzindo o custo por GB armazenado em até 80% enquanto mantém a conformidade legal para auditorias eventuais.

### Banco de Dados Transacional
* **Amazon Aurora (Compatível com PostgreSQL/MySQL):** Separação inteligente de armazenamento e computação com failover automático, alta disponibilidade em Multi-AZ e capacidade de auto-scaling de réplicas de leitura para consultas de estoque e relatórios contábeis.

### Governança e Monitoramento de Custos
* **AWS Budgets:** Criação de orçamentos mensais segregados por centro de custo (Lojas Físicas, E-commerce, Logística), emitindo alertas automatizados via Amazon SNS ao atingir 80% e 100% do teto planejado.
* **AWS Cost Explorer & AWS Cost Anomaly Detection:** Análise visual de gastos históricos e identificação proativa de desvios e cobranças atípicas.
* **Amazon CloudWatch:** Monitoramento contínuo de utilização de CPU/memória para suporte a tarefas de *rightsizing* (redimensionamento de recursos sobredimensionados).

---

## 4. Comparativo de Cenários (Antes vs. Depois)

| Critério | Infraestrutura Legada (On-Premises) | Arquitetura Proposta AWS | Impacto Financeiro / Operacional |
| :--- | :--- | :--- | :--- |
| **Modelo de Custo** | CapEx elevado (compra de servidores e licenças) | OpEx proporcional ao uso real | Fim dos gastos de capital inicial com maquinário |
| **Dimensionamento** | Superdimensionado para suportar picos | Auto Scaling elástico sob demanda | Eliminação de capacidade computacional ociosa |
| **Armazenamento de Longo Prazo** | Storages locais com expansão física cara | Amazon S3 + Ciclo de vida para S3 Glacier | Redução de até 80% no custo por Gigabyte armazenado |
| **Previsibilidade Orçamentária** | Despesas imprevistas de quebra e suporte | AWS Budgets com limites e alertas configurados | Governança proativa contra surpresas de faturamento |
| **Disponibilidade** | Ponto único de falha no data center local | Infraestrutura Multi-AZ com tolerância a falhas | Garantia de continuidade das vendas nas lojas |

---

## 5. Plano de Governança e Melhores Práticas

* **Rightsizing Periódico:** Revisão mensal de instâncias utilizando relatórios do *AWS Compute Optimizer*.
* **Rotulagem de Recursos (*Resource Tagging*):** Aplicação de tags obrigatórias (`Ambiente: Producao`, `Departamento: Farmacia`, `Filial: ID`) para distribuição transparente dos centros de custo.
* **Políticas de Desligamento:** Automação de desligamento de instâncias dos ambientes de testes e homologação durante finais de semana e madrugadas.
