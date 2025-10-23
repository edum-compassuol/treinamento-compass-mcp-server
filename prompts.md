# Prompt Create MCP Server

## Prompt Inicial

Eu quero construir um MCP Server para a API do Serviço 4Devs conforme a documentação oficial da API: @4devs-openapi.yaml
Ele gera um conjunto completo de dados para uma ou mais pessoas, incluindo nome, documentos, endereço, etc. Os dados gerados são válidos e podem ser personalizados de acordo com as opções fornecidas.

Para construir o MCP Server, utilize os seguintes passos:

1. Leia atentamente a documentação da API para o serviço:@4devs-openapi.yaml
2. Leia atentamente a documentação oficial da criação de MCP Server com Typescript: @https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/refs/heads/main/README.md
3. Com essas informações em mãos construa o servidor MCP, nomeando ele como "4devs-mcp-server".
4. Teste o MCP Server criado, utilizando o Inspector CLI do MCP Server, conforme a documentação oficial: @https://github.com/modelcontextprotocol/inspector
5. Ao final, crie um repositório git local com o código do MCP Server recém criado e comite o código.

Crie um plano detalhado para a construção do MCP Server, seguindo os passos acima, incluindo a estrutura de pastas e arquivos necessários, bem como as dependências que serão utilizadas.

## Prompt Avançado

Melhore o projeto criado com os seguintes passos:

1. Crie um dockerfile para o MCP Server criado, conforme exemplo abaixo.
2. Melhore o README.md do MCP Server criado:
   1. Apresentando as feramentas
   2. Inclua informações de build e run do dockerfile
   3. Adicionando exemplos de uso
   4. Instruções detalhadas de instalação e configuração tanto com npx quanto com docker
   5. Utilize como exemplo o README.md do MCP Server do Github: @https://raw.githubusercontent.com/github/github-mcp-server/refs/heads/main/README.md
3. Crie um Resource no MCP Server com o README.md atualizado e disponibilize.
4. Ao final, comite as alterações no repositório git local.

### Exemplo de Dockerfile para MCP Server

```Dockerfile
FROM node:22.12-alpine AS builder

COPY src/memory /app
COPY tsconfig.json /tsconfig.json

WORKDIR /app

RUN --mount=type=cache,target=/root/.npm npm install

RUN --mount=type=cache,target=/root/.npm-production npm ci --ignore-scripts --omit-dev

FROM node:22-alpine AS release

COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

ENV NODE_ENV=production

WORKDIR /app

RUN npm ci --ignore-scripts --omit-dev

ENTRYPOINT ["node", "dist/index.js"]

```

Crie um plano detalhado para a melhoria do MCP Server, seguindo os passos acima, incluindo a estrutura de pastas e arquivos necessários, bem como as dependências que serão utilizadas.

## Prompt Deploy

Para a realização do deploy, siga os passos abaixo:

1. Crie um repositório remoto no Github (Use o Github MCP Server disponível no Reasoning do AI Cockpit) para o repositório local do MCP Server criado e o adicione como repositório remoto.
2. Atualize o package.json do MCP Server com as informações complementares abaixo e comite.
3. Crie o arquivo .npmrc na raiz do repositório local com o conteúdo informado abaixo e comite.
4. Crie um arquivo para o Github Actions na pasta .github/workflows com o nome publish.yml, com o conteúdo do para realizar o publish do npm e da imagem docker no Github Packages.
   1. Para o publish do pacote npm, leia a documentação oficial do Github: @https://docs.github.com/pt/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#publishing-a-package-using-an-action
   2. Para o publish da imagem docker, leia a documentação oficial do Github: @https://docs.github.com/pt/actions/tutorials/publish-packages/publish-docker-images#publicar-imagens-em-github-packages
5. Faça o push do repositório git local para o repositório remoto no Github.
6. No repositório remoto no Github, acesse a aba Actions e verifique se o workflow criado está rodando corretamente.
7. Após o workflow rodar com sucesso, verifique na aba Packages do repositório remoto no Github se o pacote npm e a imagem docker foram publicados corretamente.
8. Teste o servidor MCP no Reasoning do AI Cockpit, conforme roteiro.

### Informações complementares para o package.json

Inclua os seguintes campos no package.json do MCP Server:

- no campo "name", inclua o prefixo contendo '@' seguido do nome de usuário no Github, por exemplo: "@seu-usuario/nome-mcp-server"
- Verique se o campo "private" está como false, caso esteja como true, altere para false.
- melhore a descrição do campo "description" com uma descrição simples de uma linha do MCP Server, com base no seu README.md.
- verifique se o campo "scripts" possui o script "inspector" com o comando "npx @modelcontextprotocol/inspector build/index.js", caso não tenha, inclua-o.
- inclua o campo "author" com os dados do usuário do Github (nome, email e url do perfil), por exemplo:

  ```json
  "author": {
    "name": "Seu Nome",
    "email": "seu@email.com",
    "url": "suaurl.com"
  }
  ```

- inclua o campo ""publishConfig" com o registry do Github Packages, por exemplo:

  ```json
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  }
  ```

- inclua o campo "repository" com o link do repositório remoto no Github, por exemplo:

  ```json
  "repository": {
    "type": "git",
    "url": "https://github.com/seu-usuario/nome-mcp-server.git"
  }
  ```

- inclua o campo "bugs" com o link do issue do repositório remoto no Github, por exemplo:

  ```json
  "bugs": {
    "url": "https://github.com/seu-usuario/nome-mcp-server/issues"
  }
  ```

### Conteúdo do arquivo .npmrc

Crie o arquivo `.npmrc` na raiz do repositório local com o seguinte conteúdo, substituindo `seu-usuario` pelo seu nome de usuário no Github:

  ```txt
    @seu-usuario:registry=https://npm.pkg.github.com
  ```

### Roteiro para testar o MCP Server

Nesse roteiro, você irá testar o MCP Server criado, no Reasoning do AI Cockpit. Você irá testar os dois pacotes, tanto o do npm quanto o da imagem docker.

1. Comece carregando e lendo o README.md do MCP Server criado, para entender as funcionalidades e como utilizá-lo.
2. Leia o arquivo de configuração dos servidores MCP existentes no Reasoning do AI Cockpit, para entender como configurar o MCP Server criado.
3. Utilize as melhores práticas para os comandos do terminal do usuário e sistema operacional.
4. Teste o MCP Server utilizando o pacote npm:
   1. Configure o MCP Server do pacote npm no arquivo de configuração dos servidores MCP conforme as instruções do README.md.
   2. Utilize as funcionalidades do MCP Server conforme descrito no README.md, realizando algumas requisições de teste.
   3. Ao final remova a configuração do MCP Server do arquivo de configuração dos servidores MCP.
5. Teste o MCP Server utilizando a imagem docker:
   1. Configure o MCP Server da imagem docker no arquivo de configuração dos servidores MCP conforme as instruções do README.md.
   2. Utilize as funcionalidades do MCP Server conforme descrito no README.md, realizando algumas requisições de teste.
   3. Ao final remova a configuração do MCP Server do arquivo de configuração dos servidores MCP.

Crie um plano detalhado para o deploy do MCP Server, seguindo os passos acima, incluindo a estrutura de pastas e arquivos necessários, bem como as dependências que serão utilizadas.