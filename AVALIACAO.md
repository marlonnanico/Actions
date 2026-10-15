\# Relatório de Avaliação - GitHub Actions \& Cloudflare D1



\## Tarefa 3 - Revisão de Código com IA

\- \*\*URL do PR com comentário da IA:\*\* https://github.com/marlonnanico/Actions/pull/5

\- \*\*Apontamento da IA:\*\* A IA identificou a ausência de parametrização `prepare().bind()` na consulta SQL, risco de injeção de SQL e a falta de índices na coluna `nome`.

\- \*\*URL do Log com Erro de Permissão (403):\*\* COLE\_AQUI\_O\_LINK\_DO\_LOG\_QUE\_FALHOU



\## Tarefa 4 - Migrações Cloudflare D1

\- \*\*URL do Run do Workflow de Migração:\*\* COLE\_AQUI\_O\_LINK\_DA\_PAGINA\_DO\_PRINT\_ACIMA



\---



\## Perguntas Teóricas



\### 1. Por que secrets em `pull\_request` de forks podem ser um risco de segurança?

Se um workflow for acionado por um Pull Request originado de um \*fork\* público sem restrições, utilizadores mal-intencionados podem alterar os ficheiros de workflow para ler e expor os \*secrets\* (chaves de API, tokens de acesso) nos logs públicos do repositório. Por isso, por padrão, o GitHub Actions não compartilha secrets em eventos `pull\_request` originados de forks de terceiros.



\### 2. Por que o token `GITHUB\_TOKEN` padrão costuma ter permissões restritas?

Princípio do menor privilégio. Caso o repositório ou uma ação de terceiros seja comprometida, as permissões restritas impedem que atacantes alterem definições críticas do repositório, façam push em branches protegidas ou apaguem recursos. Permissões adicionais (como `pull-requests: write`) devem ser declaradas explicitamente no ficheiro do workflow.



\### 3. Qual a diferença entre os eventos `pull\_request` e `push` (em `main`) no contexto de CI/CD?

O evento `pull\_request` serve para Integração Contínua (CI): executa verificações de segurança, testes estáticos e revisões antes do código ser aceito na branch principal. O evento `push` na branch `main` representa a Entrega/Implantação Contínua (CD): executa tarefas de alteração de estado no ambiente final/produção, como aplicar migrações no banco de dados e atualizar o site.



\### 4. O que aconteceria se a migração falhasse no meio da execução? Como garantir resiliência?

Em um banco relacional como o D1, se uma instrução no script de migração falhar, as alterações pendentes podem ser interrompidas. Para garantir resiliência, as migrações devem ser organizadas em ficheiros individuais versionados (ex: `0001\_...sql`, `0002\_...sql`), executadas dentro de transações atómicas (`BEGIN TRANSACTION; ... COMMIT;`), e o pipeline deve ser projetado para interromper imediatamente em caso de erro (exit code != 0), evitando a execução de scripts subsequentes.

