---
layout: post
title: "Monitoramento de Sistemas Linux: Comandos Essenciais"
date: 2025-01-23
categories: [tecnologia, administração de sistemas]
---

# Monitoramento de Sistemas Linux: Comandos Essenciais

Dominar ferramentas de monitoramento é crucial para administradores de sistemas Linux. Conheça os comandos fundamentais para diagnosticar desempenho, redes e recursos:

## 🖥️ 1. iostat (I/O Statistics)
**Função**: Monitora CPU e operações de disco  
**Instalação**:
```bash
# Debian/Ubuntu
sudo apt install sysstat

# Red Hat/CentOS
sudo yum install sysstat
```
**Uso**:

```bash

iostat -x 1 5  # Detalhes de I/O, atualiza a cada 1s (5 vezes)
```
## Saída-chave:

- **%util**: % de tempo do disco ocupado

- **await**: Tempo médio de espera por I/O (ms)

- **tps**: Transações por segundo

## 🌐 2. netstat (Network Statistics)
**Função**: Exibe conexões de rede e portas
**Instalação**:

```bash
# Debian/Ubuntu
sudo apt install net-tools
```
### Comandos úteis:

```bash
netstat -tuln    # Portas TCP/UDP em escuta
netstat -s       # Estatísticas por protocolo
netstat -r       # Tabela de roteamento
```
### Alternativa moderna:

```bash
ss -tunlp  # Equivalente com melhor performance
```
## 👥 3. w (Who & What)
**Função**: Mostra usuários logados e processos
**Exemplo**:

```bash
w  # Exibe: usuário, terminal, tempo idle, processo atual
```
## 📊 4. sar (System Activity Reporter)
**Função**: Coleta histórica de desempenho
**Uso**:

```bash
sar -u 2 5      # CPU a cada 2s (5 amostras)
sar -r 1 3      # Memória
sar -d -p 1 3   # Disco por dispositivo
```
## 🧩 5. ps (Process Status)
**Função**: Lista processos ativos
**Comando essencial**:

```bash
ps aux --sort=-%cpu | head  # Top 10 processos por CPU
```
**Campos importantes**:

- **PID**: ID do processo

- **%CPU**: Uso de CPU

- **%MEM**: Uso de memória

- **STAT**: Estado do processo

## 🚀 6. top (Task Manager)
**Função**: Monitoramento em tempo real
Atalhos dentro do top:
|---|
- **P**: Ordenar por CPU

- **M**: Ordenar por memória

- **k**: Matar processo

- **1**: Mostrar CPUs individuais

## 🧠 7. vmstat (Virtual Memory Stats)
**Função**: Analisa memória, processos e I/O
**Uso**:

```bash
vmstat 1  # Atualiza a cada 1 segundo
```
**Saída crítica*:

- ```si/so```: Swap in/out (alto = problema)

- ```us/sy```: % CPU user/system

- ```free```: Memória livre (KB)

## 💾 8. df (Disk Free)
**Função**: Espaço em sistemas de arquivos
**Exemplo**:

```bash
df -hT  # Legível + tipos de filesystem
```
## 📁 9. du (Disk Usage)
**Função**: Uso de espaço por diretório
**Comandos**:

```bash
du -sh /var/log  # Resumo do diretório
du -h --max-depth=1 /home  # Top-level
```
🧠 10. free (Memory Usage)
**Função**: Exibe uso de RAM e swap
**Uso**:

```bash
free -m  # Exibe em MB
free -h  # Formato legível
```
## Fluxo de Diagnóstico Rápido

```graph TD
    A[Problema] --> B{w/ ou top?}
    B -->|Usuários| C[w]
    B -->|Processos| D[top/ps]
    A --> E{Disco lento?}
    E -->|Sim| F[iostat]
    E -->|Espaço| G[df/du]
    A --> H{Rede?}
    H -->|Conexões| I[netstat/ss]
    H -->|Estatísticas| J[sar]
    A --> K{Memória?}
    K --> L[free/vmstat]
```
## Dicas Profissionais
- 1.Log histórico: Configure ```sar``` para coleta diária (editando ```/etc/cron.d/sysstat```)

- 2.Monitoramento contínuo:

```bash
watch -n 2 'df -h; echo; free -h'  # Atualiza a cada 2s
```
- 3.Combinações poderosas:

```bash
# Top 5 processos consumindo memória
ps aux --sort=-%mem | head -6

# Conexões ESTABLISHED por IP
netstat -tun | grep 'ESTAB' | awk '{print $5}' | cut -d: -f1 | sort | uniq -c
```
**Importante**: Para troubleshooting, sempre comece com ```w```, ```top``` e ```free``` - dão visão geral imediata do sistema.
