---
layout: post
title: "Variaves"
date: 2024-12-29
categories: [tecnologia, código aberto]
---

## Variáveis de Ambiente

As variáveis de ambiente são pares de chave-valor que armazenam informações sobre o ambiente do sistema operacional e influenciam o comportamento de processos em execução. Elas são amplamente utilizadas em sistemas operacionais Unix/Linux e também em outros sistemas operacionais como Windows.

### Variáveis de Sistema

- **PATH:** Contém uma lista de diretórios onde o sistema procura executáveis.
- **HOME:** O diretório home do usuário atual.
- **USER:** O nome do usuário atual.
- **SHELL:** O shell padrão do usuário (por exemplo, `/bin/bash`).
- **ROOT:** Diretório raiz do sistema.
- **HOSTNAME:** Nome do host (computador).

### Variáveis de Ambiente do Usuário

- **EDITOR:** Editor de texto padrão.
- **VISUAL:** Editor de texto visual.
- **PAGER:** Pagina dor de saída.
- **LANG:** Idioma e codificação de caracteres.
- **TZ:** Fuso horário.

### Como Funcionam as Variáveis de Ambiente

#### Escopo

As variáveis de ambiente podem ser definidas para o sistema inteiro ou por um usuário específico. Variáveis definidas em um terminal ou sessão de shell são geralmente locais a essa sessão.

#### Acesso

Você pode acessar o valor de uma variável de ambiente usando o prefixo `$`. Por exemplo, para acessar o diretório home do usuário, você pode usar `$HOME`.

#### Herança

Quando um processo é iniciado, ele herda as variáveis de ambiente do processo pai. Isso significa que se você definir uma variável em um terminal, todos os programas iniciados a partir desse terminal terão acesso a essa variável.

### Como Visualizar Variáveis de Ambiente

Para visualizar todas as variáveis de ambiente definidas no seu sistema, você pode usar o comando `printenv` ou `env` no terminal:

Para visualizar uma variável específica, você pode usar o comando `echo`:

```bash
echo $NOME_DA_VARIAVEL

