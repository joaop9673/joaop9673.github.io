---
layout: post
title: "Construa Seu GitHub Pessoal Offline: Git Server com Criptografia e VPN no Linux"
date: 2025-07-07
categories: [segurança, administração, linux]
---

# Construa Seu GitHub Pessoal Offline: Git Server com Criptografia e VPN no Linux

![Servidor Git com VPN e Criptografia](https://images.unsplash.com/photo-1558494945-7a658f0f6a77?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&h=600&q=80)  
*Transforme um HD externo em um servidor Git privado e seguro*

## Materiais Necessários
- HD externo (500GB+ recomendado)
- Conta Mullvad VPN
- Computador Linux (Fedora XFCE testado)

## Passo 1: Preparando o HD com Criptografia
```bash
# Particionar (substitua /dev/sdX pelo seu dispositivo)
sudo fdisk /dev/sdX  # Crie uma partição Linux (tipo 83)

# Criptografar com LUKS
sudo cryptsetup luksFormat /dev/sdX1
sudo cryptsetup open /dev/sdX1 git_hd

# Formatar como ext4 (melhor para Git)
sudo mkfs.ext4 /dev/mapper/git_hd -L "GitSecure"

# Montar
sudo mkdir /mnt/git_repos
sudo mount /dev/mapper/git_hd /mnt/git_repos
```
## Passo 2: Configurando o Servidor Git
Crie um script ```/mnt/git_repos/init_server.sh```:
```bash
#!/bin/bash

# Criar usuário dedicado
sudo useradd -m -d /mnt/git_repos/git-user git-user

# Configurar SSH
sudo mkdir /mnt/git_repos/git-user/.ssh
sudo touch /mnt/git_repos/git-user/.ssh/authorized_keys

# Permissões seguras
sudo chown -R git-user:git-user /mnt/git_repos/git-user
sudo chmod 700 /mnt/git_repos/git-user/.ssh
sudo chmod 600 /mnt/git_repos/git-user/.ssh/authorized_keys

# Iniciar servidor SSH em porta alternativa
sudo /usr/sbin/sshd -f /mnt/git_repos/sshd_config
```
Crie ```/mnt/git_repos/sshd_config```:
```
Port 49222
ListenAddress 10.64.0.1  # IP interno da Mullvad
PermitRootLogin no
PasswordAuthentication no
AuthorizedKeysFile .ssh/authorized_keys
Subsystem sftp internal-sftp
AllowUsers git-user
```

## Passo 3: Integração com Mullvad VPN
```bash
# Instalar Mullvad CLI
curl -Lo mullvad.deb https://mullvad.net/download/app/deb/latest
sudo apt install ./mullvad.deb

# Conectar e configurar
mullvad account login XXXXXXXX
mullvad relay set location br saw
mullvad lan set allow
```
Verifique o IP interno:
```
mullvad status
# Anote o IP (ex: 10.64.0.1)
```

## Passo 4: Acesso Remoto Seguro

- 1. Adicione sua chave SSH:

```bash
# Copiar chave pública para o servidor
echo "ssh-ed25519 AAAAC3Nz..." | sudo tee -a /mnt/git_repos/git-user/.ssh/authorized_keys
```
- 2. Clonar repositório remotamente:

```bash
git clone ssh://git-user@10.64.0.1:49222/mnt/git_repos/meu-projeto.git
```
## Passo 5: Startup Automático
- 1. Crie um serviço systemd ```/etc/systemd/system/git-hd.service```:

```ini
[Unit]
Description=Git Server on External HD
Requires=multi-user.target
After=mullvad-daemon.service

[Service]
Type=oneshot
ExecStart=/bin/bash /mnt/git_repos/init_server.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
- 2. Ative com:

```bash
sudo systemctl enable git-hd.service
```
## 🔄 Fluxo de Trabalho com VPN

```sequenceDiagram
    Remote Device->>+Mullvad VPN: Conecta (WireGuard)
    Mullvad VPN-->>-Remote Device: Atribui IP 10.64.0.2
    Remote Device->>Git Server (10.64.0.1): git push/pull
    Git Server-->>HD Externo: Armazena dados criptografados
```
## 💡 Recursos Adicionais
### Monitoramento:
```bash
watch -n 5 "mullvad status; sudo du -sh /mnt/git_repos/*"
```
### Backup Incremental:
```bash
# Usar borgbackup com criptografia
borg init --encryption=repokey /backup/git-repos
borg create /backup/git-repos::'{now}' /mnt/git_repos
```
### Acesso via Mobile:
Use Termux + Mullvad App para git pull no celular

## ⚠️ Dicas de Segurança
Sempre desmonte antes de remover o HD:

```bash
sudo umount /mnt/git_repos
sudo cryptsetup close git_hd
```
- 1. Configure firewall básico:

```bash
sudo ufw allow from 10.64.0.0/10 to any port 49222
sudo ufw deny 22/tcp
```
- 2. Ative auditoria:

```bash
sudo auditctl -w /mnt/git_repos -p war -k git_hd_access
```
Aviso Legal: Tutorial para fins educacionais. Verifique leis locais antes de implementar.

## Referências Técnicas Essenciais

### 1. Criptografia e Segurança
- 📚 **[LUKS Documentation](https://gitlab.com/cryptsetup/cryptsetup/-/wikis/home)**  
  Guia oficial de criptografia de disco no Linux
- 🔐 **[NIST Special Publication 800-111](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-111.pdf)**  
  Padrões de criptografia para armazenamento de dados

### 2. Git e Versionamento
- 💻 **[Pro Git Book](https://git-scm.com/book/en/v2)**  
  Referência completa sobre servidores Git e workflows
- ⚙️ **[Git Protocols Deep Dive](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols)**  
  Explicação técnica dos protocolos SSH para Git

### 3. VPN e Segurança de Rede
- 🌐 **[Mullvad VPN Documentation](https://mullvad.net/en/help/)**  
  Configuração avançada e recursos de segurança
- 🛡️ **[WireGuard Protocol White Paper](https://www.wireguard.com/papers/wireguard.pdf)**  
  Fundamentos do protocolo VPN usado pelo Mullvad

### 4. Automação e Serviços Linux
- ⚡ **[Systemd Service Units](https://www.freedesktop.org/software/systemd/man/systemd.service.html)**  
  Documentação oficial para criação de serviços
- 🔥 **[Advanced SSH Configuration](https://man.openbsd.org/sshd_config)**  
  Manual completo do OpenSSH Server

### 5. Backup e Recuperação
- 💾 **[BorgBackup Documentation](https://borgbackup.readthedocs.io/)**  
  Guia de backup criptografado com deduplicação
- 🚨 **[Disaster Recovery Best Practices](https://csrc.nist.gov/publications/detail/sp/800-34/rev-1/final)**  
  Padrões NIST para recuperação de dados

### 6. Firewall e Monitoramento
- 🔥 **[UFW Master Guide](https://help.ubuntu.com/community/UFW)**  
  Firewall simplificado para Linux
- 👁️ **[Linux Audit Framework](https://linux-audit.com/)**  
  Monitoramento de segurança empresarial

### 7. Padrões de Conformidade
- 📜 **[ISO/IEC 27001:2022](https://www.iso.org/standard/27001)**  
  Padrão internacional para segurança da informação
- 🔍 **[CIS Linux Benchmarks](https://www.cisecurity.org/cis-benchmarks/)**  
  Configurações seguras para servidores Linux

> **Nota**: Todas referências foram verificadas em Junho/2025. Links sujeitos a alterações futuras.
