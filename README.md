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

# Solicitação de Mudança: RFC-001

**IC afetado:**  
Banco de Dados – MySQL

**Versão atual:**  
MySQL 8.4

**Versão proposta:**  
MySQL 9.0

**Motivo da mudança:**  
A versão MySQL 8.4 apresentou problemas de desempenho no ambiente do Sistema de Pedidos. A mudança tem como objetivo atualizar o banco de dados para a versão MySQL 9.0, buscando melhorar o desempenho e manter o ambiente atualizado.

**Riscos:**  
- Incompatibilidade entre a aplicação e a nova versão do MySQL;
- Erros em consultas SQL;
- Alterações no comportamento de comandos ou recursos utilizados pelo sistema;
- Possível indisponibilidade do sistema durante a atualização;
- Risco de perda ou corrupção de dados caso ocorra alguma falha durante o processo.

**Impacto na aplicação:**  
A aplicação poderá apresentar falhas caso existam consultas, bibliotecas ou configurações incompatíveis com o MySQL 9.0. Por esse motivo, será necessário validar todas as funcionalidades que utilizam o banco de dados antes da implantação em produção.

**Ambientes afetados:**  
- Ambiente de desenvolvimento;
- Ambiente de testes/homologação;
- Ambiente de produção.

**Testes necessários:**  
1. Realizar backup completo do banco de dados antes da atualização;
2. Instalar o MySQL 9.0 inicialmente em ambiente de testes;
3. Verificar a conexão da aplicação com o banco de dados;
4. Testar as operações de cadastro, consulta, alteração e exclusão de pedidos;
5. Executar as principais consultas SQL utilizadas pela aplicação;
6. Realizar testes de desempenho;
7. Verificar logs da aplicação e do banco de dados;
8. Confirmar que não ocorreram erros ou perda de dados.

**Plano de implementação:**  
1. Registrar e aprovar formalmente a RFC-001;
2. Realizar backup completo do banco de dados MySQL 8.4;
3. Preparar um ambiente de testes com MySQL 9.0;
4. Restaurar uma cópia dos dados no ambiente de testes;
5. Executar os testes de compatibilidade e funcionamento da aplicação;
6. Corrigir possíveis incompatibilidades encontradas;
7. Após a aprovação dos testes, agendar a atualização do ambiente de produção;
8. Realizar novo backup antes da alteração em produção;
9. Atualizar o MySQL 8.4 para MySQL 9.0;
10. Executar testes de validação após a atualização;
11. Monitorar o funcionamento da aplicação e do banco de dados;
12. Registrar a alteração realizada e atualizar a baseline do sistema.

**Plano de rollback:**  
Caso sejam encontrados erros graves após a atualização, a mudança deverá ser revertida. O MySQL 9.0 será removido e o ambiente deverá retornar para o MySQL 8.4. O backup realizado antes da mudança deverá ser restaurado, garantindo que o sistema retorne ao estado definido pela baseline anterior. Após a restauração, deverão ser realizados testes para confirmar o funcionamento normal da aplicação.

**Responsável:**  
Equipe de Infraestrutura / Banco de Dados.

**Aprovação:**  
Pendente de avaliação e aprovação pelo responsável pelo controle de mudanças.

## Fluxo da mudança

**Solicitar → Avaliar impacto → Aprovar/Rejeitar → Implementar/Testar → Verificar/Encerrar**

1. **Solicitar:** registrar formalmente a necessidade de atualização do MySQL;
2. **Avaliar impacto:** analisar riscos, compatibilidade e possíveis impactos na aplicação;
3. **Aprovar/Rejeitar:** decidir se a mudança poderá ser realizada;
4. **Implementar/Testar:** realizar a atualização inicialmente em ambiente controlado e executar os testes;
5. **Verificar/Encerrar:** validar os resultados, registrar a mudança e, se tudo estiver correto, encerrar a RFC.

## Desafio 4 — Nova Baseline v1.1
# Baseline v1.1 — Sistema de Pedidos

## Identificação

* **Baseline:** v1.1
* **Baseline anterior:** v1.0
* **Data de criação:** 17/08/2026
* **Responsável pela aprovação:** Grupo 7 — Gabriel, Ellen e Tales

## Objetivo da Baseline

Registrar o novo estado oficial, estável e aprovado do Sistema de Pedidos após a implementação e validação da mudança solicitada pela RFC-001, estabelecendo uma nova referência para o controle de configurações e mudanças futuras.

## Itens de Configuração (ICs)

| IC             | Configuração                              |
| -------------- | ----------------------------------------- |
| Sistema        | Sistema de Pedidos — Versão 1.1           |
| Aplicação      | Node.js 22; Express 5.1; Porta 3000       |
| Banco de Dados | **MySQL 9.0; Banco: pedidos; Porta 3306** |
| Infraestrutura | Ubuntu Server 24.04; 4 GB RAM; 2 vCPUs    |
| Código-fonte   | Branch: main; Commit: abc123              |

## Evolução da Configuração

* **Baseline anterior:** v1.0
* **Mudança aprovada:** MySQL 8.4 → MySQL 9.0
* **RFC relacionada:** RFC-001
* **Resultado:** Mudança implementada e testada com sucesso
* **Nova baseline:** v1.1

## Observação

Esta baseline representa o novo conjunto completo e aprovado dos Itens de Configuração após a mudança controlada. A única alteração em relação à baseline v1.0 foi a atualização do Banco de Dados de MySQL 8.4 para MySQL 9.0. Os demais ICs permanecem inalterados.

A baseline v1.0 permanece como registro histórico do estado anterior, garantindo a rastreabilidade da evolução da configuração do sistema.

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

A baseline é importante porque estabelece um estado oficial, estável e aprovado do sistema, servindo como referência para a equipe. Ela garante rastreabilidade, facilita o controle de mudanças e permite identificar diferenças entre o estado esperado e o estado real do ambiente. Sem controle de configuração, alterações podem causar incompatibilidades, falhas, perda de estabilidade e **Configuration Drift**, dificultando também a identificação da causa dos problemas e a recuperação do sistema.


<!-- Ellen preenche -->
