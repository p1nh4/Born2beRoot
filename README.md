*This project has been created as part of the 42 curriculum by davidos-.*

# Born2beRoot

## Descrição

Born2beRoot é um projeto de administração de sistemas do currículo da 42. O objetivo é configurar um servidor Linux seguro dentro de uma máquina virtual, seguindo regras específicas de particionamento, gerenciamento de usuários, políticas de segurança e monitoramento.

O projeto abrange conceitos fundamentais de administração de servidores: partições criptografadas com LVM, configuração de firewall, endurecimento do SSH, políticas de sudo e um script de monitoramento personalizado.

---

## Instruções

### Requisitos

- VirtualBox (ou UTM no Apple Silicon)
- Debian (versão estável mais recente)

### Acessando a VM

Conecte via SSH a partir da máquina host (após configurar o port forwarding no VirtualBox, Host: 4241 → Guest: 4242):

```bash
ssh <usuario>@localhost -p 4241
```

### Verificando a configuração

```bash
sudo aa-status                          # AppArmor ativo
sudo ufw status                         # Regras do firewall
sudo systemctl status ssh               # Serviço SSH
sudo bash /usr/local/bin/monitoring.sh  # Script de monitoramento
sudo systemctl status fail2ban          # Fail2ban ativo
sudo fail2ban-client status sshd        # Estado da jail SSH
```

---

## Recursos

As referências completas — documentação, artigos e tutoriais — estão listadas em [resources.md](./resources.md).

Os três guias principais usados como base para o bonus foram:
- [Born2beRoot Bonus - Gitbook](https://noreply.gitbook.io/born2beroot/bonus-services/wordpress)
- [Born2beRoot - Vikingu-del](https://github.com/Vikingu-del/Born2beRoot/tree/main/photos)
- [Born2beRoot - mcombeau](https://github.com/mcombeau/Born2beroot/blob/main/guide/bonus_debian.md)

### Uso de IA

A IA foi utilizada nas seguintes tarefas:
- Organização e redação do `README.md`
- Esclarecimento de conceitos: AppArmor, LVM, políticas de sudo
- Análise de logs do Fail2ban e verificação de regras UFW durante a configuração do bonus

---

## Descrição do Projeto

### Escolha do Sistema Operacional: Debian

O Debian foi escolhido em vez do Rocky Linux pelos seguintes motivos:

- Grande comunidade e documentação extensa, especialmente para iniciantes em administração de sistemas
- Utiliza o gerenciador de pacotes `apt`/`dpkg`, amplamente usado em diversos ambientes de servidor
- O `AppArmor` é suportado nativamente e mais fácil de configurar em comparação com o SELinux

**Debian vs Rocky Linux:**

| | Debian | Rocky Linux |
|---|---|---|
| Base | Independente | Red Hat / CentOS |
| Gerenciador de pacotes | apt / .deb | dnf / .rpm |
| Módulo de segurança | AppArmor | SELinux |
| Firewall | UFW | firewalld |
| Comunidade | Muito grande | Focada em enterprise |
| Recomendado para | Uso geral, iniciantes | Ambientes corporativos |

### AppArmor vs SELinux

Ambos são sistemas de Controle de Acesso Obrigatório (MAC) que restringem o que os programas podem fazer, adicionando uma camada de segurança além das permissões Unix tradicionais.

- **AppArmor** funciona com base em caminhos de arquivo. Cada programa possui um perfil que define quais arquivos e capacidades pode acessar. O kernel verifica as políticas do AppArmor antes de executar qualquer processo. Mais simples de configurar.
- **SELinux** funciona com base em rótulos atribuídos a cada arquivo, processo e recurso. É mais granular e oferece garantias de segurança mais fortes, mas é significativamente mais complexo de configurar. Padrão em sistemas Red Hat.

### UFW vs firewalld

- **UFW (Uncomplicated Firewall)** é uma interface para o `iptables` projetada para simplificar o gerenciamento de firewall em Debian. As regras são diretas e fáceis de entender.
- **firewalld** é usado em Rocky/RHEL. Suporta alterações dinâmicas sem reiniciar e utiliza o conceito de "zonas" para organizar regras.

Neste projeto, o UFW foi configurado com a porta 4242 (SSH) na parte obrigatória. No bonus, a porta 8080 (WordPress) foi adicionada.

### VirtualBox vs UTM

- **VirtualBox** é um hipervisor gratuito e multiplataforma da Oracle que suporta x86/x64. Amplamente utilizado em ambientes educacionais com boas funcionalidades de snapshots e port forwarding via NAT.
- **UTM** é utilizado em Macs com Apple Silicon (M1/M2), pois o VirtualBox não suporta ARM nativamente. O UTM utiliza o QEMU internamente.

O VirtualBox foi utilizado neste projeto em um host Linux x86.

### Principais Decisões de Design

**Particionamento:** Partições criptografadas com LVM. O LVM adiciona uma camada de abstração entre os discos físicos e o sistema de arquivos, permitindo redimensionar e agrupar partições sem perda de dados.

**Políticas de segurança:** Política de senha estrita (expiração a cada 30 dias, mínimo de 10 caracteres, maiúsculas, minúsculas e números obrigatórios, máximo de 3 caracteres idênticos consecutivos). Sudo limitado a 3 tentativas, com todas as ações registradas em `/var/log/sudo/`.

**Gerenciamento de usuários:** Usuário não-root pertencente aos grupos `user42` e `sudo`. Login SSH como root desabilitado.

**Serviços:** SSH apenas na porta 4242. UFW ativo na inicialização. Script `monitoring.sh` executado a cada 10 minutos via cron.

**Porta 8080 para o WordPress:** A porta 80 requer privilégios root no Linux (portas abaixo de 1024 são reservadas ao sistema). Como o lighttpd é executado sem privilégios root por segurança, foi utilizada a porta 8080.

---

## Bonus

### WordPress (lighttpd + MariaDB + PHP)

Site WordPress funcional configurado com lighttpd como servidor web, MariaDB como base de dados e PHP para processamento dinâmico. Acessível via porta 8080.

### Fail2ban

O Fail2ban foi escolhido como serviço adicional por complementar diretamente a configuração SSH do projeto, protegendo contra ataques de força bruta. Monitoriza os logs de autenticação e bloqueia automaticamente IPs que excedam o número máximo de tentativas.

Configuração aplicada (`/etc/fail2ban/jail.local`):

```
[sshd]
enabled  = true
maxretry = 3
findtime = 10m
bantime  = 1d
port     = 4242
logpath  = %(sshd_log)s
backend  = %(sshd_backend)s
```

O funcionamento foi testado manualmente: após 3 tentativas de login SSH com senha errada, o IP foi automaticamente banido e o evento registado em `/var/log/fail2ban.log`.
