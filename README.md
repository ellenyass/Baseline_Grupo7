# Atividade Prática — Baseline
## Gerência de Configuração e Controle de Mudanças

**Grupo 7** — Gabriel, Ellen e Tales


## Desafio 1 — Baseline v1.0

Baseline inicial criada em `BASELINE-v1.0.md`, representando o estado oficial
aprovado do Sistema de Pedidos.

## Desafio 2 — Mudança não autorizada


**1. A baseline foi alterada? Por quê?**
Não. O documento `BASELINE-v1.0.md` continua registrando MySQL 8.4, pois nenhuma
nova baseline foi formalmente criada. O que ocorreu foi uma divergência entre o
estado documentado (baseline) e o estado real do ambiente em produção — a baseline
em si permanece a mesma até que um novo processo formal a substitua.

**2. Qual Item de Configuração (IC) foi modificado?**
O IC de **Banco de Dados**, que passou de MySQL 8.4 para MySQL 9.0 sem
atualização correspondente na documentação da baseline.

**3. Essa alteração deveria ter sido realizada diretamente em produção?**
Não. Mudanças em ICs críticos, como o banco de dados, não devem ser aplicadas
diretamente em produção sem antes passar por um processo formal de avaliação,
testes e aprovação, justamente pelo risco de incompatibilidades — como os erros
de consulta que passaram a ocorrer após a atualização.

**4. Qual processo deveria ter sido executado antes da alteração?**
O fluxo de controle de mudanças: **Solicitar → Avaliar impacto → Aprovar/Rejeitar
→ Implementar/Testar → Verificar/Encerrar**. Isso incluiria abrir uma RFC,
avaliar riscos de compatibilidade entre a aplicação e a nova versão do MySQL,
testar em ambiente controlado e só então aprovar a ida para produção.

**5. O que deve acontecer com a baseline após uma mudança aprovada?**
Após uma mudança formalmente aprovada, testada e validada, uma **nova baseline**
deve ser criada (neste caso, a v1.1), refletindo o novo estado oficial do
sistema — agora com MySQL 9.0 — e tornando-se a nova referência de conformidade.

## Configuration Drift no incidente

<!-- Gabriel preenche -->

## Desafio 3 — RFC

<!-- Tales preenche -->

## Desafio 4 — Nova Baseline v1.1

<!-- Ellen preenche -->

## Desafio 5 — Configuration Drift (tabela)

<!-- Gabriel preenche -->

## Pergunta final

<!-- Ellen preenche -->