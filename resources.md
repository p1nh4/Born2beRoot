# Recursos — Born2beRoot

## Máquinas Virtuais

**O que é uma VM:**
- [O que é uma Máquina Virtual? - Azure](https://azure.microsoft.com/pt-pt/resources/cloud-computing-dictionary/what-is-a-virtual-machine/)
- [O que é uma Máquina Virtual? - AWS](https://aws.amazon.com/pt/what-is/virtual-machine/)

**Propósitos de uma VM:**
Permitem executar um sistema isolado dentro da máquina principal. Útil para testar software, duplicar ambientes e usar em diferentes máquinas
sem afetar o sistema principal. Em servidores, se o host principal parar, basta migrar as VMs para outra máquina.
- [Por que usar VMs? - Reddit](https://www.reddit.com/r/homelab/comments/qw4bqn/why_use_virtual_machines/?tl=pt-br)

---

## Escolha do Sistema Operacional

Debian foi escolhido pela facilidade e suporte da comunidade. Para ambientes enterprise seria preferível algo baseado em CentOS/Red Hat.
A diferença principal: Rocky/CentOS usa base Red Hat com pacotes `dnf`/`.rpm`, enquanto o Debian usa `apt`/`.deb`.
- [Debian vs Rocky para servidores - Reddit](https://www.reddit.com/r/homelab/comments/1cscznh/debian_vs_rocky_linux_for_my_server/?tl=pt-br)

---

## apt vs aptitude

`apt` é a versão atualizada do `apt-get` via linha de comandos, mais rápido e simples. `aptitude` tem uma interface (TUI) mais amigável,
mais adequado para gestão de sistemas maiores.
- [apt vs apt-get - AWS](https://aws.amazon.com/pt/compare/the-difference-between-apt-and-apt-get/)
- [Visão geral do aptitude - YouTube](https://www.youtube.com/watch?v=stWpR_KpGD8)
- [apt vs aptitude - Reddit](https://www.reddit.com/r/debian/comments/ymuu6l/what_is_the_difference_between_apt_aptget_and/?tl=pt-br)
- [Debian Handbook - apt](https://debian-handbook.info/browse/pt-BR/stable/sect.apt-get.html)

---

## AppArmor

Restringe a execução dos programas com base no caminho do ficheiro, funciona como uma cerca elétrica. O kernel verifica as permissões
do AppArmor antes de executar qualquer processo.
- [AppArmor - Debian Handbook](https://debian-handbook.info/browse/pt-BR/stable/sect.apparmor.html)
- [Visão geral do AppArmor - DioLinux](https://plus.diolinux.com.br/t/apparmor-uma-camada-extra-de-seguranca-para-o-linux/69091)

---

## LVM

Cria uma camada de abstração entre os discos físicos e o sistema de arquivos, permitindo redimensionar partições e agrupar múltiplos
discos em pools, sem necessidade de formatar ou perda de dados.
- [LVM - Arch Wiki (PT)](https://wiki.archlinux.org/title/LVM_(Portugu%C3%AAs))
- [Como funciona o LVM? - Reddit](https://www.reddit.com/r/linux4noobs/comments/8gvj0j/how_does_lvm_work_exactly/?tl=pt-pt)

---

## MariaDB

- [mysql_secure_installation - SuperUser](https://superuser.com/questions/364086/error-message-sudo-mysql-secure-installation-command-not-found)

---

## Guias de referência para o Bonus

Projetos base consultados para a configuração:
- [Born2beRoot Bonus - Gitbook](https://noreply.gitbook.io/born2beroot/bonus-services/wordpress)
- [Born2beRoot - Vikingu-del](https://github.com/Vikingu-del/Born2beRoot/tree/main/photos)
- [Born2beRoot - mcombeau](https://github.com/mcombeau/Born2beroot/blob/main/guide/bonus_debian.md)


