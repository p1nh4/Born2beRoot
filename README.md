*This project has been created as part of the 42 curriculum by davidos-.*

# Born2beRoot

## Descrição

Born2beRoot é um projeto de administração de sistemas do currículo da 42. O objetivo é configurar um servidor Linux seguro dentro de uma máquina virtual, seguindo regras específicas de particionamento, gerenciamento de usuários, políticas de segurança e monitoramento.

O projeto abrange conceitos fundamentais de administração de servidores: partições criptografadas com LVM, configuração de firewall, endurecimento do SSH, políticas de sudo e um script de monitoramento personalizado.

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

O Rocky é mais complexo de configurar e voltado para ambientes corporativos. O Debian é mais leve e amigável para iniciantes, mantendo-se adequado para produção.

### AppArmor vs SELinux

Ambos são sistemas de Controle de Acesso Obrigatório (MAC) que restringem o que os programas podem fazer, adicionando uma camada de segurança além das permissões Unix tradicionais.

- **AppArmor** funciona com base em caminhos de arquivo e é geralmente mais fácil de configurar. Cada programa possui um perfil que define quais arquivos e capacidades pode acessar. O kernel verifica as políticas do AppArmor antes de executar qualquer processo.
- **SELinux** funciona com base em rótulos atribuídos a cada arquivo, processo e recurso. É mais granular e difícil de configurar, mas oferece garantias de segurança mais fortes. Usado por padrão em sistemas baseados em Red Hat.

### UFW vs firewalld

- **UFW (Uncomplicated Firewall)** é uma interface para o `iptables` projetada para simplificar o gerenciamento de firewall em sistemas baseados em Debian. As regras são diretas e fáceis de entender.
- **firewalld** é utilizado em sistemas Rocky/RHEL. Suporta alterações dinâmicas de regras sem reiniciar o firewall e utiliza o conceito de "zonas" para organizar as regras.

Neste projeto, o UFW foi utilizado com apenas a porta 4242 aberta para acesso SSH. No bonus, as portas 8080 (WordPress) foram adicionadas às regras do UFW conforme necessário.

### VirtualBox vs UTM

- **VirtualBox** é um hipervisor gratuito e multiplataforma da Oracle que suporta arquiteturas x86/x64. É amplamente utilizado em ambientes educacionais e possui boas funcionalidades de snapshots e port forwarding via NAT.
- **UTM** é utilizado em Macs com Apple Silicon (M1/M2), pois o VirtualBox não suporta ARM nativamente. O UTM utiliza o QEMU internamente.

O VirtualBox foi utilizado neste projeto em um host Linux x86.

### Principais Decisões de Design

**Particionamento:** Foram criadas partições criptografadas usando LVM. O LVM (Logical Volume Manager) adiciona uma camada de abstração entre os discos físicos e o sistema de arquivos, permitindo redimensionar e agrupar partições sem perda de dados ou necessidade de reformatação.

**Políticas de segurança:** Foi implementada uma política de senha estrita (expiração a cada 30 dias, mínimo de 10 caracteres, letras maiúsculas, minúsculas e números obrigatórios, máximo de 3 caracteres idênticos consecutivos). O acesso via sudo é limitado a 3 tentativas, com registro de todas as ações em `/var/log/sudo/`.

**Gerenciamento de usuários:** Um usuário não-root pertencente aos grupos `user42` e `sudo` foi criado. O login SSH como root está desabilitado.

**Serviços:** O SSH roda apenas na porta 4242. O firewall UFW está ativo na inicialização com apenas a porta 4242 permitida. Um script bash `monitoring.sh` é executado a cada 10 minutos via cron, transmitindo informações do sistema para todos os terminais.

**Porta 8080 para o WordPress:** A porta 80 requer privilégios de root no Linux (portas abaixo de 1024 são reservadas ao sistema). Como o lighttpd é executado sem privilégios root por segurança, foi utilizada a porta 8080, que é a alternativa padrão para servidores web em ambientes não-root.

---

## Bonus

### WordPress (lighttpd + MariaDB + PHP)

Foi configurado um site WordPress funcional utilizando lighttpd como servidor web, MariaDB como base de dados e PHP para processamento dinâmico. O WordPress é acessível via porta 8080, adicionada às regras do UFW.

### Fail2ban

O Fail2ban foi escolhido como serviço adicional por ser uma medida de segurança diretamente complementar ao SSH já configurado no projeto. Ele protege contra ataques de força bruta, monitorizando os logs de falhas de autenticação e bloqueando automaticamente IPs que excedam o número máximo de tentativas.

Configuração aplicada na jail `sshd`:
- `maxretry = 3` — bloqueio após 3 tentativas falhadas
- `findtime = 10m` — janela de tempo para contar tentativas
- `bantime = 1d` — duração do bloqueio
- `port = 4242` — porta SSH do projeto

Para verificar o estado:
```bash
$ sudo fail2ban-client status
$ sudo fail2ban-client status sshd
$ sudo tail -f /var/log/fail2ban.log
```

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

Verificar se o AppArmor está rodando:
```bash
sudo aa-status
```

Verificar o status do UFW:
```bash
sudo ufw status
```

Verificar o serviço SSH:
```bash
sudo systemctl status ssh
```

Verificar o script de monitoramento:
```bash
sudo bash /usr/local/bin/monitoring.sh
```

Verificar o Fail2ban:
```bash
sudo systemctl status fail2ban
sudo fail2ban-client status sshd
```

---

## Recursos

### Máquinas Virtuais
- [O que é uma Máquina Virtual? - Azure](https://azure.microsoft.com/pt-pt/resources/cloud-computing-dictionary/what-is-a-virtual-machine/)
- [O que é uma Máquina Virtual? - AWS](https://aws.amazon.com/pt/what-is/virtual-machine/)
- [Por que usar VMs? - Reddit](https://www.reddit.com/r/homelab/comments/qw4bqn/why_use_virtual_machines/?tl=pt-br)

### Debian vs Rocky
- [Debian vs Rocky para servidores - Reddit](https://www.reddit.com/r/homelab/comments/1cscznh/debian_vs_rocky_linux_for_my_server/?tl=pt-br)

### apt vs aptitude
- [apt vs apt-get - AWS](https://aws.amazon.com/pt/compare/the-difference-between-apt-and-apt-get/)
- [Visão geral do aptitude - YouTube](https://www.youtube.com/watch?v=stWpR_KpGD8)
- [apt vs aptitude - Reddit](https://www.reddit.com/r/debian/comments/ymuu6l/what_is_the_difference_between_apt_aptget_and/?tl=pt-br)
- [Debian Handbook - apt](https://debian-handbook.info/browse/pt-BR/stable/sect.apt-get.html)

### AppArmor
- [AppArmor - Debian Handbook](https://debian-handbook.info/browse/pt-BR/stable/sect.apparmor.html)
- [Visão geral do AppArmor - DioLinux](https://plus.diolinux.com.br/t/apparmor-uma-camada-extra-de-seguranca-para-o-linux/69091)

### LVM
- [LVM - Arch Wiki (PT)](https://wiki.archlinux.org/title/LVM_(Portugu%C3%AAs))
- [Como funciona o LVM? - Reddit](https://www.reddit.com/r/linux4noobs/comments/8gvj0j/how_does_lvm_work_exactly/?tl=pt-pt)

### MariaDB
- [mysql_secure_installation - SuperUser](https://superuser.com/questions/364086/error-message-sudo-mysql-secure-installation-command-not-found)

### Guias de referência para o Bonus
- [Born2beRoot Bonus - WordPress/lighttpd/MariaDB - Gitbook](https://noreply.gitbook.io/born2beroot/bonus-services/wordpress)
- [Born2beRoot - Vikingu-del (fotos e referência visual)](https://github.com/Vikingu-del/Born2beRoot/tree/main/photos)
- [Born2beRoot - mcombeau (guia completo bonus Debian)](https://github.com/mcombeau/Born2beroot/blob/main/guide/bonus_debian.md)

### Uso de IA

A IA foi utilizada nas seguintes tarefas:
- Organização e redação do `README.md`: estruturação das secções, revisão do português e formatação em Markdown
- Esclarecimento de conceitos durante o projeto: diferenças entre AppArmor/SELinux, funcionamento do LVM e políticas de sudo
- Depuração de configurações: análise de logs do Fail2ban e verificação de regras UFW
