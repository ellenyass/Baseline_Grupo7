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