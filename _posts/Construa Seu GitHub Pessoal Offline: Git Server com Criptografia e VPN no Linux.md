# Construa Seu GitHub Pessoal Offline: Git Server com Criptografia e VPN no Linux

![Servidor Git com VPN e Criptografia](https://images.unsplash.com/photo-1558494945-7a658f0f6a77?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&h=600&q=80)  
*Transforme um HD externo em um servidor Git privado e seguro*

## Materiais Necessários
- HD externo (500GB+ recomendado)
- Conta Mullvad VPN
- Computador Linux (Fedora XFCE testado)

## Passo 1: Preparando o HD com Criptografia
```bash
sudo cryptsetup luksFormat /dev/sdX1
sudo cryptsetup open /dev/sdX1 git_hd
sudo mkfs.ext4 /dev/mapper/git_hd -L "GitVault"
sudo mkdir /mnt/git_repos
sudo mount /dev/mapper/git_hd /mnt/git_repos
```
## Passo 2: Configurando o Servidor Git
```bash
sudo useradd -m -d /mnt/git_repos/git-user git-user
sudo mkdir /mnt/git_repos/git-user/.ssh
sudo touch /mnt/git_repos/git-user/.ssh/authorized_keys
sudo chown -R git-user:git-user /mnt/git_repos/git-user
sudo chmod 700 /mnt/git_repos/git-user/.ssh
sudo chmod 600 /mnt/git_repos/git-user/.ssh/authorized_keys
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI..." | sudo tee -a /mnt/git_repos/git-user/.ssh/authorized_keys
sudo -u git-user mkdir /mnt/git_repos/meu-projeto.git
sudo -u git-user git init --bare /mnt/git_repos/meu-projeto.git
```
## Passo 3: Integração com Mullvad VPN
```bash
curl -Lo mullvad.deb https://mullvad.net/download/app/deb/latest
sudo apt install ./mullvad.deb
mullvad account login SEU_CODIGO_DE_28_DIGITOS
mullvad lan set allow
mullvad status
```
## Passo 4: Configuração do SSH Server
Crie ```/mnt/git_repos/sshd_config```:

```apache
Port 49222
ListenAddress 10.64.215.112
PermitRootLogin no
PasswordAuthentication no
AuthorizedKeysFile .ssh/authorized_keys
Subsystem sftp internal-sftp
AllowUsers git-user
```
Inicie o servidor:

```bash
sudo /usr/sbin/sshd -f /mnt/git_repos/sshd_config
```
## Passo 5: Acessando Seu Repositório
```bash
git clone ssh://git-user@10.64.215.112:49222/mnt/git_repos/meu-projeto.git
```
### Automação com Systemd
Crie ```/etc/systemd/system/git-hd.service```:

```ini
[Unit]
Description=Git Server on Encrypted HD
Requires=mullvad-daemon.service
After=network.target

[Service]
ExecStart=/usr/sbin/sshd -f /mnt/git_repos/sshd_config
Restart=always
User=git-user

[Install]
WantedBy=multi-user.target
```
Ative o serviço:

```bash
sudo systemctl enable --now git-hd.service
```
### Dicas de Segurança Avançada
```bash
sudo ufw allow from 10.64.0.0/10 to any port 49222
sudo ufw deny 22/tcp
sudo auditctl -w /mnt/git_repos -p war -k git_hd_access
sudo apt install borgbackup
borg init --encryption=repokey /caminho/backup/git-repos
borg create /caminho/backup/git-repos::'{now}' /mnt/git_repos
```
### Solução de Problemas Comuns
Problema: Conexão SSH recusada

```bash
mullvad lan set allow
sudo ufw allow from 10.64.0.0/10
```
Problema: Permissões negadas

```bash
sudo chown -R git-user:git-user /mnt/git_repos
```
Problema: HD não reconhecido

```bash
sudo cryptsetup open /dev/sdX1 git_hd
sudo mount /dev/mapper/git_hd /mnt/git_repos
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
