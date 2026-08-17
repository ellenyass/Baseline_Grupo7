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
testes e aprovação, justamente pelo risco de incompatibilidades, como os erros
de consulta que passaram a ocorrer após a atualização.

**4. Qual processo deveria ter sido executado antes da alteração?**
O fluxo de controle de mudanças: **Solicitar → Avaliar impacto → Aprovar/Rejeitar
→ Implementar/Testar → Verificar/Encerrar**. Isso incluiria abrir uma RFC,
avaliar riscos de compatibilidade entre a aplicação e a nova versão do MySQL,
testar em ambiente controlado e só então aprovar a ida para produção.

**5. O que deve acontecer com a baseline após uma mudança aprovada?**
Após uma mudança formalmente aprovada, testada e validada, uma **nova baseline**
deve ser criada (neste caso, a v1.1), refletindo o novo estado oficial do sistema agora com MySQL 9.0 e tornando-se a nova referência de conformidade.

## Configuration Drift no incidente

A atualização do MySQL 8.4 para 9.0 diretamente em produção, sem passar pelo
processo de controle de mudanças, gerou um caso de **Configuration Drift**: o
ambiente real passou a divergir do estado documentado na baseline v1.0.

- **Estado esperado (baseline v1.0):** MySQL 8.4
- **Estado real (produção):** MySQL 9.0

### Por que essa divergência causou erros

Upgrades de versão major em bancos de dados frequentemente trazem mudanças de
comportamento padrão, deprecação de sintaxe antiga e alterações na forma como
o driver de conexão da aplicação se comunica com o banco. Como a aplicação
Node.js/Express foi desenvolvida e testada assumindo o comportamento do MySQL
8.4, qualquer uma dessas mudanças pode gerar falhas em consultas específicas —
o que explica por que os erros apareceram em "algumas consultas", e não em
todas: o drift raramente quebra o sistema por completo de forma imediata, ele
degrada de forma parcial e imprevisível até que o ponto de incompatibilidade
seja exercitado.

### Por que isso é um risco mesmo além do banco

O impacto do drift não se limita à instabilidade técnica. A partir do momento
em que o ambiente real diverge da baseline documentada, a própria baseline
deixa de ser confiável como fonte de verdade: qualquer pessoa que consultar
`BASELINE-v1.0.md` para investigar um problema, treinar alguém novo no time
ou planejar uma mudança futura estará partindo de uma informação desatualizada.
O drift, portanto, não compromete apenas o item de configuração alterado — ele
corrói a confiabilidade de toda a documentação de configuração do sistema.

### Como o drift é resolvido

O drift permanece até que uma das duas coisas aconteça: (a) o ambiente seja
revertido para bater com a baseline v1.0, ou (b) a mudança seja formalizada
através de uma RFC e uma nova baseline (v1.1) seja criada para refletir o
novo estado real como oficial — restaurando a coerência entre documentação e
ambiente.

## Desafio 3 — RFC

<!-- Tales preenche -->

## Desafio 4 — Nova Baseline v1.1

<!-- Ellen preenche -->

## Desafio 5 — Configuration Drift (tabela)

## Desafio 5 — Configuration Drift

| Situação | É mudança controlada? | Está na baseline? |
|---|---|---|
| Desenvolvedor altera o código e realiza um novo commit | Sim, desde que siga o fluxo de revisão (PR/code review) | Só se o commit for formalmente incorporado a uma nova baseline aprovada |
| Administrador altera manualmente uma configuração em produção | Não, pois não passou por RFC, avaliação de impacto ou testes | Não — representa um desvio (drift) em relação ao estado oficial documentado |
| Mudança aprovada e documentada gera a baseline v1.1 | Sim, ja que seguiu o fluxo completo de controle de mudanças | Sim, este é o próprio processo de criação de uma nova baseline |

### E se alguém alterar o servidor manualmente depois da v1.1?

O mesmo problema se repetiria: o ambiente voltaria a divergir do estado
documentado, agora em relação à baseline v1.1. Isso mostra que Configuration
Drift não é um evento isolado, é um risco permanente sempre que existe a
possibilidade de alterar produção fora do processo formal. Cada baseline nova
não "resolve" o risco de drift, apenas reinicia o ponto de referência contra
o qual futuras divergências serão medidas.

## Pergunta final

<!-- Ellen preenche -->