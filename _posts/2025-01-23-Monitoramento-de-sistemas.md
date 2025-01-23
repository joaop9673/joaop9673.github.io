---
layout: post
title: "Monitoramento de sistemas"
date: 2025-01-23
categories: [tecnologia, código aberto]
---

#### Monitoramento de Sistema: Comandos Essenciais para Administradores Linux
No mundo da administração de sistemas Linux, é fundamental ter ferramentas que ajudem a monitorar o desempenho e a saúde do sistema. Neste post, vamos explorar alguns comandos essenciais: **iostat**, **netstat**, **w**, **sar** e **ps**. Cada um deles oferece informações valiosas sobre diferentes aspectos do sistema.

## 1. iostat

O comando **iostat** é ultilizado para monitorar a ultilização da CPU e a atividade de entrada/saida (**I**/**O**) dos sispositivos. Ele e parte do pacote **sysstat** e fornece estatiticas que ajudam a indetificar gargalos de desempenho.

### Uso Basico
```bash
iostat -x 1
```

- `-x`: Exibe estatísticas detalhadas.
- `1`: Atualiza a saída a cada 1 segundo.

### Saída

A saída do `iostat` inclui informações como:

- `%user`: Porcentagem de tempo que a CPU está ocupada com processos de usuário.
- `%iowait`: Porcentagem de tempo que a CPU está ociosa, aguardando operações de I/O.

## 2. netstat

O comando `netstat` é uma ferramenta poderosa para monitorar conexões de rede, tabelas de roteamento e estatísticas de interface. Ele é útil para diagnosticar problemas de rede e verificar o estado das conexões.

### Uso Básico

```bash
netstat -tuln
```

- `-t`: Exibe conexões TCP.
- `-u`: Exibe conexões UDP.
- `-l`: Mostra apenas os sockets que estão escutando.
- `-n`: Exibe endereços e números de porta em formato numérico.

### Saída

A saída do `netstat` inclui informações como:

- Endereço local e remoto.
- Estado da conexão (LISTEN, ESTABLISHED, etc.).

## 3. w

O comando `w` fornece uma visão geral dos usuários conectados ao sistema e suas atividades. É uma maneira rápida de ver quem está logado e o que estão fazendo.

### Uso Básico

```bash
w
```

### Saída

A saída do `w` inclui:

- Nome do usuário.
- Tempo de login.
- Atividade atual (comando em execução).
- Tempo de inatividade.

## 4. sar

O comando `sar` (System Activity Report) é uma ferramenta abrangente para coletar e relatar informações sobre a atividade do sistema. Ele pode monitorar CPU, memória, I/O, rede e muito mais.

### Uso Básico

```bash
sar -u 1
```

- `-u`: Relata a utilização da CPU.
- `1`: Atualiza a saída a cada 1 segundo.

### Saída

A saída do `sar` inclui informações detalhadas sobre:

- `%user`, `%system`, `%idle`: Percentuais de uso da CPU.
- Estatísticas de I/O e memória.

## 5. ps

O comando `ps` é utilizado para exibir informações sobre os processos em execução no sistema. É uma ferramenta essencial para monitorar e gerenciar processos.

### Uso Básico

```bash
ps aux
```

- `a`: Exibe processos de todos os usuários.
- `u`: Exibe informações detalhadas sobre os processos.
- `x`: Inclui processos que não estão associados a um terminal.

### Saída

A saída do `ps` inclui:

- ID do processo (PID).
- Usuário que iniciou o processo.
- Uso de CPU e memória.
- Tempo de execução.

