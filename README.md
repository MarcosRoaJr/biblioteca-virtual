# Biblioteca Virtual — Marcos Roa Junior

> Atividade em desenvolvimento para reproduzir uma **biblioteca virtual**.  
> Os primeiros passos começaram pela modelagem e implementação do **banco de dados**.

##  Objetivo
Construir a base de dados e a estrutura mínima para gerenciar **livros, autores, gêneros, exemplares, usuários** e **empréstimos**, garantindo **integridade referencial** e um fluxo simples de cadastro e controle.

## Tecnologias
- **MariaDB 10.4+** ou **MySQL 8+**
- Charset e collation: `utf8mb4 / utf8mb4_general_ci`

## Estrutura do Banco (tabelas)
- `autor` — dados do autor (com `nome_completo` gerado)
- `genero` — lista de gêneros (com `UNIQUE` no nome)
- `usuarios` — leitores/usuários (com `UNIQUE` em `email` e `cpf`)
- `livro` — dados do livro
- `exemplares` — cópias físicas do livro (único por livro + número do exemplar)
- `livro_autor` — relação N:N entre livro e autor
- `livro_genero` — relação N:N entre livro e gênero
- `emprestimo` — histórico de empréstimos (depende de `exemplares` e `usuarios`)

Relações principais:
- `exemplares.id_livro → livro.id_livro` (CASCADE)
- `emprestimo.id_exemplares → exemplares.id_exemplares` (CASCADE)
- `emprestimo.id_usuario → usuarios.id_usuario` (CASCADE)
- `livro_autor.id_livro → livro.id_livro` (CASCADE)
- `livro_autor.id_autor → autor.id_autor` (RESTRICT)
- `livro_genero.id_livro → livro.id_livro` (CASCADE)
- `livro_genero.id_genero → genero.id_genero` (RESTRICT)

## Decisões de modelagem
- **Enums** para status de `emprestimo` e `exemplares` (clareza e validação simples).
- **Coluna gerada** `autor.nome_completo` evita redundância manual.
- **Índices** em campos de busca e FKs melhoram performance.
- **UNIQUE em email/cpf** evita duplicidades em usuários.
- **CASCADE** onde faz sentido operacional (excluir livro apaga exemplares e empréstimos ligados).
