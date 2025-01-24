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

### Instalando o `iostat`

Antes de usar o `iostat`, você pode precisar instala-lo. Em distribuições baseadas em Debian, como o Ubuntu, você pode instar com o seguinte comando:

```bash
sudo apt install sysstat
```

Para distribuições baseadas em Red Hat, como o CentOS,fedora ou Rhel

```bash
sudo yum install sysstat
```
### Usando o `iostat`

A sintaxe basica do comando `iostat`e:


```bash
iostat [opções] [instervalo] [contagem]
```
- **Opções**:Parâmetros que modificam a saida do comando.
- **Intervalo**:O tempo em segundos entre as atualizações da saida.
- **Contagem**:O numero de vezes que a saida deve ser atualizado.

## Exemplo de uso

```bash
iostat -x 1 5
```

- `-x`: Exibe estatísticas detalhadas sobre a ultilização de I/O.
- `1`: Atualiza a saída a cada 1 segundo.
- `5`: Exiber as saida 5 vezes.

### Saída

A saída do `iostat` pode ser dividida em duas seções principais:a utilização da CPU e a atividade de I/O dos dispositivos.

#### 1. Utilização da CPU

A primeira parte da saida mostra a utilização da CPU, com colunas como:

- `%user`: Porcentagem de tempo que a CPU está ocupada com processos de usuário.
- `%system`: Porcentagem de tempo que a CPU esta ocupada com processos de usuario.
- `%idle`: Porcentagem de tempo que a CPU esta ociosa.
- `%iowait`: Porcentagem de tempo que a CPU está ociosa, aguardando operações de I/O.

#### 2. Atividade de I/O
A segunda parte da saida mostra  informações sobre os dispositivos de armazenamento,com colunas como:

- `Device`: Nome do dispositivo.
- `tps`: Transações por segundo (numero de operações de I/O).
- `KB_read/s`: Kilobytes lidos por segundo.
- `KB_wrtn/s`: Kilobytes escritos por segundo.
- `await`: Tempo medio de espera para operações de I/O(em millissegundos).
- `%util`: Porcentagem de tempo que o dispositivo esta ocupado com operações de I/O.

#### Exemplos Praticos

### Monitorando a Utilização de CPU e I/O

para mostra a utilização da CPU e a atividade de I/O em tempo real, você pode usar:

```bash
iostat -x 2
```

Isso atualizara a saida a cada 2 segundos, permitindo que quem estiver operando veja como o desempenho de sistema muda ao longo do tempo.

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

## 6. top

O comando `top` fornece uma visão em tempo real dos processos em execução nos sistema, mmostrando indformações sobre o uso de CPU, memoria e outros recuros.

### Uso Basico

```bash
top
```
A saida do `top` inclui:

- Lista de processos em execução, ordenados pelo uso de CPU.
- Informações sobre a carga do sistema, uso de memoria e swap.
- Permite interações, como matar processos ou alterar prioridades 

## 7. vmstat

O comando `vmstat` (Virtual Memory Statics) fornece informações sonbre processos, memorias, e paginação, block I/O, traps e atividades da CPU.

### Uso Basico

```bash
vmstat 1
```

- `1`: Atualiza a saida a cada 1 segundo.

A saida do `vmstat` inclui:

- Informações sobre a memoria livre e usada.
- Estatisticas de I/O e CPU.

## 8. df

O comando `df`(Disk Free)e usado para relatar a quantidade de espaço em disco disponivel em sistmas de arquivos.

### Uso Basico
```bash
df -h
```
- `-h`:Exibe os tamanhos em um formato legivel(Human-readable).

a saida do `df` inclui:

- Sistema de arquivos.
- Tamanho total, usado e dispomivel.
- Ponto de mantagem.

## 9. du

O comando `du`(Disk Usage)e ultilizado para estimar o uso de espaço em disco de arquivos e diretorios.

### Uso Basico
```bash
du -sh /caminho/do/diretorio
```

- `-s`: Resumo(summary)do uso de espaço.
- `-h`:Fomato legivel pra humanos

A saida do `du` mostra o espaço total usado pelo diretorio especifico.

## 10. free

O comando `free` exibe infomações sobre a memoria do sistema, incluido memoria total, usada, livre e swap.

### Uso Basico 

```bash
free -h
```

- `-h`:Exibe os tamanhos em um formato levivel pra humanos

A saida do `free`inclui:

- Total, usado e livre de memoria RAM e swap.





























