
Oque é uma VM:
	https://azure.microsoft.com/pt-pt/resources/cloud-computing-dictionary/what-is-a-virtual-machine/
	https://aws.amazon.com/pt/what-is/virtual-machine/

Propositos de uma VM:
	Executar um computador na mesma maquina, com isso podemos testar sotfware,
	duplicar a maquina e usar em diferentes maquinas. Tudo isso isolado do sistema
	principal. Caso de servidores, caso o principal pare, podemos so ligar as VMs
	em outra maquina, e caso quebre a VMs nao havera problema no sistema principal.

	https://www.reddit.com/r/homelab/comments/qw4bqn/why_use_virtual_machines/?tl=pt-br

Escolha de S.O
	Debian devido a facilidade e suporte da comunidade, se quisesse algo mais enterprise
	escolheria algo relacionado a base de CentOS.
	Diferenca com CentOS, um vem da base de Red Hat, o Debian usa pacotes
	apt e .deb

	https://www.reddit.com/r/homelab/comments/1cscznh/debian_vs_rocky_linux_for_my_server/?tl=pt-br

Diferencas aptitude e apt
	apt seria a versao atualizada de apt-get via linha de comandos, e aptitude seria um instalador
	que vem com uma interface TUI mais amigavel para instalar pacotes. O primeiro e mais rapido
	e simples o outro para sistemas mais grandes.

	https://aws.amazon.com/pt/compare/the-difference-between-apt-and-apt-get/
	https://www.youtube.com/watch?v=stWpR_KpGD8
	https://www.reddit.com/r/debian/comments/ymuu6l/what_is_the_difference_between_apt_aptget_and/?tl=pt-br
	https://debian-handbook.info/browse/pt-BR/stable/sect.apt-get.html

Oque e AppArmor
	Ele restringe como uma cerca eletrica a execucao dos programas de acordo a onde esta localizado.
	O kernel vai vef. cada vez no AppArmor as permissoes dadas antes de executar aquele processo.

	https://debian-handbook.info/browse/pt-BR/stable/sect.apparmor.html
	https://plus.diolinux.com.br/t/apparmor-uma-camada-extra-de-seguranca-para-o-linux/69091

Oque e LVM
	Cria uma camada de abstracao entre os discos fisicos e sistema de arquivos permitindo
	redimensionar particoes, agrupar multiplos discos em piscinas, sem necessidade de formatar
	ou perda de dados (flexivel)

	https://wiki.archlinux.org/title/LVM_(Portugu%C3%AAs)
	https://www.reddit.com/r/linux4noobs/comments/8gvj0j/how_does_lvm_work_exactly/?tl=pt-pt

Secure installation mysql
	https://superuser.com/questions/364086/error-message-sudo-mysql-secure-installation-command-not-found

projetos baseados:
	https://noreply.gitbook.io/born2beroot/bonus-services/wordpress
	https://github.com/Vikingu-del/Born2beRoot/tree/main/photos
	https://github.com/mcombeau/Born2beroot/blob/main/guide/bonus_debian.md

tester:
	https://github.com/gemartin99/Born2beroot-Tester
