# Guia de Fluxo de Trabalho Git: Estrutura de Branches com Controle de PRs

## Estrutura de Branches

Neste fluxo de trabalho, utilizaremos as seguintes branches principais:

- `main`: Branch de produção, contendo o código estável.
- `dev`: Branch de desenvolvimento, onde todas as novas features são integradas.
- `feat/`: Branches para desenvolvimento de novas features.
- `fix/`: Branches para correções de bugs.

## Convenções de Nomenclatura

- **Features:** `feat/nome-da-feature`
- **Hotfixes:** `fix/nome-do-fix`

## Regras Gerais

1. **Commits Diretos:** Commits diretos para `main` e `dev` são proibidos. Todo código deve passar por um PR.
2. **Pull Requests (PRs):** Todo código deve ser revisado por pelo menos um outro desenvolvedor antes de ser mesclado.
3. **Revisão de Código:** É obrigatório que todos os PRs sejam aprovados e que os testes passem antes do merge.
4. **Status de Build:** Os PRs só podem ser mesclados se todos os checks de build e testes estiverem “verdes”.
5. **Proteção de Branches:** As branches `main` e `dev` são protegidas e só podem receber merges através de PRs aprovados.

## Processo de Desenvolvimento

### 1. Iniciando uma Nova Feature

1. Certifique-se de estar na branch `dev` e atualizado:

    ```bash
    git checkout dev
    git pull origin dev
    ```

2. Crie uma nova branch para a feature:

    ```bash
    git checkout -b feat/nome-da-feature
    ```

3. Desenvolva a feature, commitando suas mudanças localmente:

    ```bash
    git add .
    git commit -m "Descrição das mudanças"
    ```

4. Quando finalizar, envie sua branch para o repositório remoto:

    ```bash
    git push origin feat/nome-da-feature
    ```

5. Crie um PR para mesclar a branch `feat/nome-da-feature` na `dev`.

### 2. Revisão de PR e Merge

1. Após a criação do PR, a equipe de revisão será notificada.
2. O PR deve ser revisado e aprovado por pelo menos um revisor.
3. O PR deve passar por todos os checks de CI/CD.
4. Após a aprovação, o PR pode ser mesclado na `dev`.

### 3. Integração na Branch `main`

1. Quando uma feature ou conjunto de features na branch `dev` estiver pronto para produção, crie um PR para mesclar `dev` na `main`.
2. A branch `main` deve ser protegida com as mesmas regras de PRs.
3. O merge para a `main` deve ser feito através de um PR aprovado.

### 4. Corrigindo Bugs com Hotfixes

1. Certifique-se de estar na branch `main` e atualizado:

    ```bash
    git checkout main
    git pull origin main
    ```

2. Crie uma nova branch para o hotfix:

    ```bash
    git checkout -b fix/nome-do-fix
    ```

3. Aplique a correção, commitando suas mudanças:

    ```bash
    git add .
    git commit -m "Descrição da correção"
    ```

4. Envie sua branch para o repositório remoto:

    ```bash
    git push origin fix/nome-do-fix
    ```

5. Crie um PR para mesclar a branch `fix/nome-do-fix` na `main`.

6. Após o merge na `main`, mescle também na `dev` para manter a paridade:

    ```bash
    git checkout dev
    git pull origin dev
    git merge main
    git push origin dev
    ```

## Configurações de Proteção no GitHub

### Protegendo as Branches `main` e `dev`

1. Vá até o repositório no GitHub e acesse **Settings > Branches**.
2. Adicione regras de proteção para as branches `main` e `dev`.
3. Ative as seguintes configurações:
    - **Require a pull request before merging**
    - **Require status checks to pass before merging**
    - **Include administrators** (opcional)
    - **Restrict who can push to matching branches**

### Configuração de PRs

1. Defina um template de PR para garantir que todos os PRs contenham as informações necessárias (descrição da mudança, como testar, etc.).
2. Use GitHub Actions para rodar linters, testes, e outras verificações automáticas nos PRs.

## Exemplo de Processos

### Exemplo 1: Criando e Mesclando uma Feature

1. **Criando a feature:**

    ```bash
    git checkout dev
    git pull origin dev
    git checkout -b feat/novo-layout
    # Desenvolvimento...
    git add .
    git commit -m "Implementa novo layout na página de login"
    git push origin feat/novo-layout
    ```

2. **Criando um PR no GitHub:**
   - Crie um PR para mesclar `feat/novo-layout` em `dev`.
   - Aguarde a revisão e aprovação.
   - Após aprovação e sucesso dos testes, mescle o PR.

### Exemplo 2: Aplicando um Hotfix

1. **Criando o hotfix:**

    ```bash
    git checkout main
    git pull origin main
    git checkout -b fix/corrige-bug-login
    # Correção...
    git add .
    git commit -m "Corrige bug de autenticação no login"
    git push origin fix/corrige-bug-login
    ```

2. **Mesclando o hotfix:**
   - Crie um PR para mesclar `fix/corrige-bug-login` em `main`.
   - Após a aprovação, mescle o PR e depois mescle `main` em `dev` para manter a paridade.

## Conclusão

Este guia define um processo estruturado e controlado de desenvolvimento usando GitFlow adaptado para incorporar controle de PRs e proteger branches críticas. Seguindo este fluxo de trabalho, sua equipe poderá colaborar de maneira eficaz e garantir que o código em produção seja sempre de alta qualidade.
