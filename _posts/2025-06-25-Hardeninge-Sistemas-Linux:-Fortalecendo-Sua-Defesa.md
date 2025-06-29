---
layout: post
title: "Hardening de Sistemas Linux: Fortalecendo Sua Defesa"
date: 2025-06-25
categories: [segurança, administração, linux]
---

# Hardening de Sistemas Linux: Fortalecendo Sua Defesa
Hardening é o processo de proteger sistemas Linux contra ameaças através da redução da superfície de ataque e implementação de configurações seguras. Em um mundo onde:

- **Ataques aumentam 38% ao ano** (Fonte: Relatório IBM Security 2024)
- **95% das violações** exploram configurações inadequadas
- **Sistemas padrão** têm +200 pontos vulneráveis

## Princípios Fundamentais
1. **Mínimo privilégio**: Usuários e serviços só acessam o necessário
2. **Defesa em profundidade**: Múltiplas camadas de segurança
3. **Falha segura**: Bloqueios automáticos em erros
4. **Auditoria contínua**: Monitoramento proativo

---

## 🔧 Configurações Essenciais

### 1. Atualizações Automatizadas
```bash
# Fedora/RHEL
sudo dnf install dnf-automatic
sudo systemctl enable --now dnf-automatic.timer

# Debian/Ubuntu
sudo apt install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```
### 2. Controle de Acesso
```bash
# Bloquear logins root
sudo sed -i 's/^PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config

# Limitar usuários SSH
echo "AllowUsers usuario_admin" | sudo tee -a /etc/ssh/sshd_config

# Tempo de inatividade
echo "ClientAliveInterval 300" | sudo tee -a /etc/ssh/sshd_config
sudo systemctl reload sshd
```
### 3. Firewall Restritivo com nftables
```bash
#!/usr/sbin/nft -f
flush ruleset

table inet firewall {
    chain input {
        type filter hook input priority 0; policy drop;
        
        # Conexões estabelecidas
        ct state established,related accept
        
        # Loopback
        iifname "lo" accept
        
        # ICMP (ping)
        ip protocol icmp accept
        
        # SSH
        tcp dport 22 accept comment "SSH"
        
        # Logging
        log prefix "DROP: "
    }
    
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
    
    chain output {
        type filter hook output priority 0; policy accept;
    }
}
```
Salve como ```/etc/nftables.conf``` e ative com:
```bash
sudo systemctl enable --now nftables
```

### 4. Proteção de Memória
```bash
# Prevenir buffer overflows
echo "kernel.exec-shield = 1" | sudo tee -a /etc/sysctl.conf
echo "kernel.randomize_va_space = 2" | sudo tee -a /etc/sysctl.conf

# Proteger contra ataques de negação de serviço
echo "net.ipv4.tcp_syncookies = 1" | sudo tee -a /etc/sysctl.conf
echo "net.ipv4.conf.all.rp_filter = 1" | sudo tee -a /etc/sysctl.conf

# Aplicar configurações
sudo sysctl -p
```
## 🛡️ Ferramentas de Segurança
### 1. SELinux vs AppArmor

| Característica	    | SELinux	     | AppArmor        | 
|---------------------|--------------|-----------------|
|Complexidade	        |   Alta       | Moderada        |
|Perfis pré-definidos |	  +200	     |  +150           |
|Logs detalhados	    |  audit.log	 |syslog           |
|Distros padrão	      | RHEL, CentOS |Ubuntu, Debian   |

### Ativação SELinux (Enforcing):

```bash
sudo setenforce 1
sudo sed -i 's/SELINUX=.*/SELINUX=enforcing/' /etc/selinux/config
```
### 2. Fail2Ban - Proteção Contra Bruteforce
```bash
# Instalação
sudo apt install fail2ban

# Configuração personalizada

# /etc/fail2ban/jail.d/sshd.conf
[sshd]
enabled = true
maxretry = 3
bantime = 1h
findtime = 600
ignoreip = 192.168.1.0/24

# Reiniciar serviço
sudo systemctl restart fail2ban
```
### 3. Rkhunter - Detecção de Rootkits
```bash
# Instalar e executar verificações
sudo dnf install rkhunter chkrootkit
sudo rkhunter --update
sudo rkhunter --check --sk
sudo chkrootkit -q
```
### 4.Criar perfil AppArmor:
```sudo aa-genprof /caminho/do/app```

## 📊 Automação com Ferramentas de Compliance

### 1. Lynis - Auditoria Automatizada
```bash
# Instalação (Debian/Ubuntu)
curl -s https://packages.cisofy.com/keys/cisofy-software-public.key | sudo gpg --dearmor -o /usr/share/keyrings/cisofy-software-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/cisofy-software-archive-keyring.gpg] https://packages.cisofy.com/community/lynis/deb/ stable main" | sudo tee /etc/apt/sources.list.d/cisofy-lynis.list
sudo apt update && sudo apt install -y lynis

# Execução (todos os sistemas)
sudo lynis audit system --no-colors --quick
```
### 2. OpenSCAP - Conformidade com Padrões
```bash
# Instalação (Ubuntu)
sudo apt install -y openscap-scanner scap-security-guide

# Instalação (RHEL/CentOS)
sudo dnf install -y openscap-scanner scap-security-guide

# Verificação CIS (Ubuntu 22.04)
sudo oscap xccdf eval \
  --profile xccdf_org.ssgproject.content_profile_cis \
  --results scan-results.xml \
  --report report.html \
  /usr/share/xml/scap/ssg/content/ssg-ubuntu2204-ds.xml
```
### 3. CIS-CAT - Benchmark Profissional
```bash
# Uso básico (requer Java 11+)
./Assessor-CLI.sh -b -rs benchmark -u
```

## ⚠️ Verificação de Segurança Rápida
```bash
# SSH
sudo sshd -T | grep -E '(permitrootlogin|passwordauthentication)'

# Firewall
sudo ufw status verbose || sudo firewall-cmd --state

# MAC (SELinux/AppArmor)
sudo selinuxenabled && sudo sestatus || sudo aa-status

# Fail2Ban
sudo fail2ban-client status sshd
```
## 🧠 Hardening do Kernel (sysctl)
Arquivo: ```/etc/sysctl.d/99-hardening.conf```
```bash
# Prevenção de ataques de rede
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1

# Mitigação de exploits
kernel.kptr_restrict = 2
kernel.unprivileged_bpf_disabled = 1
kernel.dmesg_restrict = 1
kernel.perf_event_paranoid = 3

# Prevenção de IP spoofing
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
```
# Aplicar configurações
```
sudo sysctl --system
```
## 🔐 Segurança de Usuários e Permissões
### 1. Política de Senhas (PAM)
Arquivo: ```/etc/security/pwquality.conf```
```conf
minlen = 12
minclass = 4
maxrepeat = 3
dictcheck = 1
usercheck = 1
```
### 2. Restrições de Sudo Seguras
Arquivo: ```/etc/sudoers.d/secure```
```conf
# Boas práticas:
Defaults use_pty, timestamp_timeout=5, passwd_timeout=1
Defaults logfile="/var/log/sudo.log"

# Exemplo seguro para grupo admin:
%admin ALL=(ALL:ALL) SETENV ALL
```
### 3. Controle de Acesso
```bash
# Instalar dependências (RHEL)
sudo dnf install -y pam_script pam_access

# Configuração PAM (/etc/security/access.conf)
-:ALL EXCEPT root admin (tty1):ALL
```
## 🚀 Automação com Ansible
```yaml
- name: Hardening Básico
  hosts: all
  become: true
  tasks:
    - name: Instalar ferramentas de compliance
      package:
        name:
          - lynis
          - openscap-scanner
          - scap-security-guide
        state: latest

    - name: Configurar hardening do kernel
      copy:
        src: 99-hardening.conf
        dest: /etc/sysctl.d/
        mode: 0600

    - name: Aplicar configurações sysctl
      command: sysctl -p /etc/sysctl.d/99-hardening.conf

    - name: Configurar política de senhas
      copy:
        src: pwquality.conf
        dest: /etc/security/
        mode: 0644
```



## 📚 Referências Técnicas

### Documentação Oficial
1. **National Institute of Standards and Technology (NIST)**  
   [SP 800-123 - Guide to General Server Security](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-123.pdf)  
   Padrões oficiais para segurança de servidores

2. **CIS Benchmarks**  
   [Linux Benchmark v3.0.1](https://www.cisecurity.org/benchmark/linux)  
   Configurações recomendadas para hardening

3. **Linux Kernel Security Documentation**  
   [https://www.kernel.org/doc/html/latest/security](https://www.kernel.org/doc/html/latest/security)  
   Proteções de memória e controle de acesso

### Ferramentas
4. **SELinux Official Docs**  
   [https://selinuxproject.org](https://selinuxproject.org)  
   Arquitetura e políticas de segurança

5. **AppArmor Documentation**  
   [https://apparmor.net/documentation](https://apparmor.net/documentation)  
   Guia de perfis e implementação

6. **Lynis - Auditing Tool**  
   [https://cisofy.com/documentation/lynis](https://cisofy.com/documentation/lynis)  
   Manual completo de auditoria

### Estudos e Padrões
7. **IBM Security Report 2024**  
   [Cost of a Data Breach Report](https://www.ibm.com/reports/data-breach)  
   Estatísticas sobre vulnerabilidades

8. **OpenSCAP Security Guide**  
   [https://www.open-scap.org/security-policies](https://www.open-scap.org/security-policies)  
   Políticas CIS, STIG e PCI-DSS

9. **OWASP Linux Security Cheatsheet**  
   [https://cheatsheetseries.owasp.org/cheatsheets/Linux_Security_Cheat_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Linux_Security_Cheat_Sheet.html)

### Livros Especializados
10. **Linux Hardening in Hostile Networks**  
    Scott McCarty (ISBN: 978-1-80056-177-2)  
    Capítulos 4-7 sobre configuração segura

11. **Practical Security for DevOps**  
    Vincent Sesto (ISBN: 978-1-4842-7884-9)  
    Automação de hardening em pipelines CI/CD

### Comunidade
12. **Linux Hardening Project**  
    [https://github.com/linuxhardening-guide](https://github.com/linuxhardening-guide)  
    Configurações comunitárias atualizadas

13. **Red Hat Security Guides**  
    [https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/security_hardening](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/security_hardening)

### Ferramentas Adicionais
14. **Fail2Ban Documentation**  
    [https://fail2ban.org/wiki](https://fail2ban.org/wiki)  
    Configuração de proteção contra bruteforce

15. **Rkhunter User Guide**  
    [https://rkhunter.cvs.sourceforge.net](https://rkhunter.cvs.sourceforge.net)  
    Detecção de rootkits e backdoors
