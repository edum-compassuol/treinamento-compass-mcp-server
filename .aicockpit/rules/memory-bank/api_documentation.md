# Documentação da API 4Devs

Esta é a documentação para a coleção de endpoints da API 4Devs, utilizada para gerar diversos tipos de documentos e dados brasileiros.

## Visão Geral

A API expõe um único endpoint (`https://www.4devs.com.br/ferramentas_online.php`) que aceita requisições `POST`. A ação específica a ser executada é determinada pelo parâmetro `acao` enviado no corpo da requisição.

---

## 1. Gerador de Documentos de Pessoas

Este endpoint gera um conjunto completo de dados para uma ou mais pessoas, incluindo nome, documentos, endereço, etc. Os dados gerados são válidos e podem ser personalizados de acordo com as opções fornecidas.

Opções de Personalização:

- **sexo (string):** Define o sexo da(s) pessoa(s) a ser(em) gerada(s). Valores possíveis:
  - H: homem
  - I: Aleatório
  - M: feminino
- **pontuacao (string):** Indica se o número dos documentos devem incluir pontuação. Valores:
  - S: sim
  - N: não
- **idade (number):** Define a idade da(s) pessoa(s) a ser(em) gerada(s). O valor "0" (zero) gera uma idade aleatória
- **cep_estado (string):** UF de cada Estado. Não preencher para gerar cidades aleatórias de diferentes Estados.
- **txt_qtde (number):** Quantidade de pessoas a serem geradas. Deve ser um valor entre 1 e 30.
- **cep_cidade (number):** Cidade associada a UF, valor desse campo se refere ao código da cidade no 4Devs. O não preenchimento resulta em um valor aleatório. O valor deve ser recuperado na chamada de carregar código de cidades.

### Informações adicionais

- Para que a geração de documentos ocorra, o parâmetro `acao` deve ser preenchido com o valor `gerar_pessoa`. Este parâmetro determina o tipo de geração de documentos.
- Se `cep_estado` estiver vazio, `cep_cidade` também deve estar vazio.
- O campo `cep_cidade` deve ser obtido usando o endpoint ["Carregar Cidades por UF"](#2-carregar-cidades-por-uf) (ver seção 2).
- O seu valor vem do atributo `value` das tags `<option>` retornadas.

### Exemplo de Requisição

- **Método:** `POST`
- **URL:** `https://www.4devs.com.br/ferramentas_online.php`
- **Corpo da Requisição (form-data):**

| Parâmetro | Tipo | Obrigatoriedade | Valores Possíveis |
| :--- | :--- | :--- | :--- |
| `acao` | `text` | Obrigatório | `gerar_pessoa` |
| `sexo` | `text` | Obrigatório | `H` (Homem), `M` (Mulher), `I` (Indiferente/Aleatório) |
| `pontuacao`| `text` | Opcional | `S` (Sim), `N` (Não) padrão |
| `idade` | `number` | Opcional | `0` para idade aleatória, padrão |
| `cep_estado`| `text` | Opcional | Siglas de UF (ex: `SC`, `SP`) |
| `txt_qtde` | `number` | Obrigatório | Mínimo: `1`, Máximo: `30` |
| `cep_cidade`| `number` | Opcional | Um código numérico (ex: `8542`) |

### Exemplo de Resposta

- **Resposta de Sucesso (Exemplo):**
  - **Código:** `200 OK`
  - **Conteúdo:** Um array de objetos JSON, onde cada objeto representa uma pessoa.

```json
[
    {
        "nome": "Guilherme Danilo Peixoto",
        "idade": 72,
        "cpf": "985.184.119-63",
        "rg": "26.895.316-8",
        "data_nasc": "20/05/1953",
        "sexo": "Masculino",
        "signo": "Touro",
        "mae": "Gabriela Aline Fátima",
        "pai": "Thiago Igor Peixoto",
        "email": "guilherme_peixoto@tecsysbrasil.com.br",
        "senha": "dju8YADWGo",
        "cep": "89518-971",
        "endereco": "Rua Dona Maria Mendes 416",
        "numero": 390,
        "bairro": "Centro",
        "cidade": "Macieira",
        "estado": "SC",
        "telefone_fixo": "(49) 3853-1858",
        "celular": "(49) 99990-7734",
        "altura": "1,89",
        "peso": 97,
        "tipo_sanguineo": "O-",
        "cor": "vermelho"
    }
]
```

---

## 2. Carregar Cidades por UF

Este endpoint retorna uma lista de cidades para um determinado estado (UF). Esta chamada deve ser utilizada quando um usuário deseja gerar dados para uma cidade específica.

- **Método:** `POST`
- **URL:** `https://www.4devs.com.br/ferramentas_online.php`
- **Corpo da Requisição (form-data):**

| Parâmetro | Tipo | Descrição | Valores Possíveis | Obrigatoriedade |
| :--- | :--- | :--- | :--- | :--- |
| `acao` | `text` | Ação a ser executada. | `carregar_cidades` | Obrigatório |
| `cep_estado`| `text` | UF do estado. | Siglas de UF (ex: `SC`, `SP`). | Obrigatório |

- **Resposta de Sucesso (Exemplo):**
  - **Código:** `200 OK`
  - **Conteúdo:** Uma string HTML contendo tags `<option>` com o nome e o valor de cada cidade.

```html
<option value=""></option>
<option value="8319">Abdon Batista</option>
<option value="8320">Abelardo Luz</option>
<option value="8321">Agrolândia</option>
...
```

---

## 3. Gerador de Certidões

Gera um número de certidão (nascimento, casamento, etc.). Ele gera um tipo de certidão por requisição.

- **Método:** `POST`
- **URL:** `https://www.4devs.com.br/ferramentas_online.php`
- **Corpo da Requisição (form-data):**

| Parâmetro | Tipo | Descrição | Valores Possíveis | Obrigatoriedade |
| :--- | :--- | :--- | :--- | :--- |
| `acao` | `text` | Ação a ser executada. | `gerador_certidao` | Obrigatório |
| `pontuacao`| `text` | Define se o número terá pontuação. | `S` (Sim), `N` (Não) padrão | Opcional |
| `tipo_certidao`| `text` | O tipo de certidão a ser gerada. | `nascimento`, `casamento`, `casamento_religioso`, `obito`, `Indiferente` (padrão) |  Opcional |

- **Resposta de Sucesso (Exemplo):**
  - **Código:** `200 OK`
  - **Conteúdo:** Uma string com o número da certidão gerada.

```txt
294303 01 55 2015 1 23846 633 7312880-50
```

---

## 4. Gerador de CNH

Gera um número de Carteira Nacional de Habilitação (CNH).

- **Método:** `POST`
- **URL:** `https://www.4devs.com.br/ferramentas_online.php`
- **Corpo da Requisição (form-data):**

| Parâmetro | Tipo | Descrição | Valores Possíveis | Obrigatoriedade |
| :--- | :--- | :--- | :--- | :--- |
| `acao` | `text` | Ação a ser executada. | `gerar_cnh` | Obrigatório |

- **Resposta de Sucesso (Exemplo):**
  - **Código:** `200 OK`
  - **Conteúdo:** Uma string com o número da CNH gerada.

```txt
74700162719
```

---

## 5. Gerador de PIS

Gera um número de PIS.

- **Método:** `POST`
- **URL:** `https://www.4devs.com.br/ferramentas_online.php`
- **Corpo da Requisição (form-data):**

| Parâmetro | Tipo | Descrição | Valores Possíveis | Obrigatoriedade |
| :--- | :--- | :--- | :--- | :--- |
| `acao` | `text` | Ação a ser executada. | `gerar_pis` | Obrigatório |
| `pontuacao`| `text` | Define se o número terá pontuação. | `S` (Sim), `N` (Não) padrão | Opcional |

- **Resposta de Sucesso (Exemplo):**
  - **Código:** `200 OK`
  - **Conteúdo:** Uma string com o número do PIS gerado.

```txt
629.87968.36-0
```

---

## 6. Gerador de Título de Eleitor

Gera um número de Título de Eleitor para um estado específico.

- **Método:** `POST`
- **URL:** `https://www.4devs.com.br/ferramentas_online.php`
- **Corpo da Requisição (form-data):**

| Parâmetro | Tipo | Descrição | Valores Possíveis | Obrigatoriedade |
| :--- | :--- | :--- | :--- | :--- |
| `acao` | `text` | Ação a ser executada. | `gerar_titulo_eleitor` | Obrigatório |
| `estado` | `text` | UF do estado para o título. | Siglas de UF (ex: `SC`, `SP`). | Opcional |

- **Resposta de Sucesso (Exemplo):**
  - **Código:** `200 OK`
  - **Conteúdo:** Uma string com o número do Título de Eleitor gerado.

```txt
202681730949
```
