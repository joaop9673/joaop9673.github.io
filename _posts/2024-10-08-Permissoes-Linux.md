---
layout: post
title: "Controle Total: Dominando Permissões no Linux para Segurança de Dados"
date: 2024-10-08
categories: [tecnologia, segurança, linux]
---

# Controle Total: Dominando Permissões no Linux para Segurança de Dados

## O Sistema de Defesa do Linux
As permissões são o cerne da segurança no Linux, atuando como guardiãs que regulam:
- Quem pode acessar seus arquivos
- Quem pode modificá-los
- Quem pode executar programas
- Como os diretórios compartilhados se comportam

## Anatomia das Permissões: Entendendo as Peças

### Os Três Pilares
1. **Proprietário (User)**: Criador original (controla com `chown`)
2. **Grupo (Group)**: Coleção de usuários (gerencia com `chgrp`)
3. **Outros (Others)**: Demais usuários do sistema

### Os Três Poderes

| Permissão | Símbolo | Efeito em Arquivos       | Efeito em Diretórios      |
|:-----------|:---------|:--------------------------|:---------------------------|
| Leitura   | **r**   | Ver conteúdo             | Listar itens              |
| Escrita   | **w**   | Modificar/excluir        | Criar/remover arquivos    |
| Execução  | **x**   | Executar como programa   | Acessar (cd)              |

## Decodificando o `ls -l`: Seu Radar de Permissões

**Exemplo real**:

```bash
drwxr-xr-- 2 alice devs 4096 Out 10 09:30 Projetos/
-rw-r----- 1 bob devs 1024 Out 10 08:15 plano.txt
```

**Leitura do Código**:
- **Primeiro caractere**: Tipo de recurso  
  `d` = diretório, `-` = arquivo, `l` = link simbólico
- **Próximos 9 caracteres**: Permissões em 3 grupos de 3:
  - Posições 1-3: Proprietário (ex: `rwx`)
  - Posições 4-6: Grupo (ex: `r-x`)
  - Posições 7-9: Outros (ex: `r--`)
- **Campos seguintes**:
  - Contagem de links
  - Proprietário
  - Grupo
  - Tamanho
  - Data de modificação
  - Nome do recurso

## Controle Prático com `chmod`

### Método Simbólico (Instruções Verbais)
```bash
chmod u+x script.sh           # Adiciona execução para o proprietário
chmod g-w relatorio.pdf       # Remove permissão de escrita do grupo
chmod o= arquivo.conf         # Remove todas as permissões de outros
chmod a+r publico.txt         # Adiciona leitura para todos (all)
```
## Método Octal (Códigos Numéricos)
```bash
chmod 700 ~/.ssh/             # Proprietário: rwx | Grupo: --- | Outros: ---
chmod 644 config.conf         # Proprietário: rw- | Grupo: r-- | Outros: r--
chmod 750 scripts/            # Proprietário: rwx | Grupo: r-x | Outros: ---
```
Tabela de referência rápida:

Valor	Permissões	Binário
0	---	000
1	--x	001
2	-w-	010
3	-wx	011
4	r--	100
5	r-x	101
6	rw-	110
7	rwx	111


