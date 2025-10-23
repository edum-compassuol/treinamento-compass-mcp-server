# Prompt Create MCP Server

## Prompt Inicial

Eu quero construir um MCP Server para a API do Serviço 4Devs conforme a documentação oficial da API: @4devs-openapi.yaml
Ele gera um conjunto completo de dados para uma ou mais pessoas, incluindo nome, documentos, endereço, etc. Os dados gerados são válidos e podem ser personalizados de acordo com as opções fornecidas.

Para construir o MCP Server, utilize os seguintes passos:

1. Leia atentamente a documentação da API para o serviço:@4devs-openapi.yaml
2. Leia atentamente a documentação oficial da criação de MCP Server com Typescript: @https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/refs/heads/main/README.md
3. Com essas informações em mãos construa o servidor MCP, nomeando ele como "4devs-mcp-server".
4. Teste o MCP Server criado, utilizando o Inspector do MCP Server, conforme a documentação oficial: @https://github.com/modelcontextprotocol/inspector
5. Ao final, crie um repositório git local com o código do MCP Server recém criado e comite o código.

## Prompt Avançado

Melhore o projeto criado com os seguintes passos:

1. Crie um dockerfile para o MCP Server criado.
2. Melhore o README.md do MCP Server criado:
   1. Apresentando as feramentas
   2. Inclua informações de build e run do dockerfile
   3. Adicionando exemplos de uso
   4. Instruções detalhadas de instalação e configuração tanto com npx quanto com docker
   5. Utilize como exemplo o README.md do MCP Server do Github: @https://raw.githubusercontent.com/github/github-mcp-server/refs/heads/main/README.md
3. Crie um Resource no MCP Server com o README.md atualizado e disponibilize.
4. Ao final, comite as alterações no repositório git local.

## Prompt Deploy

Para a realização do deploy, siga os passos abaixo:

1. Crie um repositório remoto no Github para o repositório local do MCP Server criado e o adicione como repositório remoto.
2. Atualize o package.json do MCP Server com as informações complementares abaixo e comite.
3. Crie o arquivo .npmrc na raiz do repositório local com o conteúdo informado abaixo e comite.
4. Crie um arquivo para o Github Actions na pasta .github/workflows com o nome publish.yml, com o conteúdo do para realizar o publish do npm e da imagem docker no Github Packages.
   1. Para o publish do pacote npm, leia a documentação oficial do Github: @https://docs.github.com/pt/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#publishing-a-package-using-an-action
   2. Para o publish da imagem docker, leia a documentação oficial do Github: @https://docs.github.com/pt/actions/tutorials/publish-packages/publish-docker-images#publicar-imagens-em-github-packages
5. Faça o push do repositório git local para o repositório remoto no Github.
6. No repositório remoto no Github, acesse a aba Actions e verifique se o workflow criado está rodando corretamente.
7. Após o workflow rodar com sucesso, verifique na aba Packages do repositório remoto no Github se o pacote npm e a imagem docker foram publicados corretamente.
8. Teste o servidor MCP no Reasoning do AI Cockpit, conforme roteiro.
