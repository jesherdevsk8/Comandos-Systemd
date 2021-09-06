
## --- Lista de comandos úteis do Systemd no dia a dia do administrador de sistemas Linux:

## ---------------Principais Comandos ---------------

## ------ Veja o manual, caso queira saber algo a mais-----
man systemd

## systemd-analyze. Mostra todo o tempo de  processo de inicialização do boot, que o cuida o kernel.
systemd-analyze

## systemd-analyze blame. Mostra mais detalhes da inicialização.
systemd-analyze blame

## systemd-cgtop. É como se fosse o comando top do systemd
systemd-cgtop
systemd-cgtop --help
# caso queira saber algum comando a mais.

## systemctl status nome.servico. Para ver o status de um serviço, a fins de gerencia-lo.
systemctl status ssh

## systemctl start nome.servico. Para inicializar um serviço.
systemctl start ssh

## systemctl stop nome.servico. Para parar o serviço.
systemctl stop ssh

## systemctl restart nome.servico. Para Reiniciar um serviço.
systemctl restart ssh

## ----------- Habilitando e desabilitando serviços --------------

## systemctl disable nome.servico. Para desabilitar o serviço. obs: Executa-lo como root
systemctl disable avahi-daemon.service avahi-daemon.socket
## Isso só ocorrerá no proximo boot, então é recomendado para-lo primeiro com o comando systemctl stop nome

## systemctl enable nome.servico. Para habilitar
systemctl enable avahi-daemon.service avahi-daemon.socket

## systemctl is-enabled nome.servico. Para ver se estará desabilitado
systemctl is-enabled avahi-daemon.service

## Caso queira ver o arquivo, a fins de saber sobre a diferença de service e socket
systemctl cat avahi-daemon.service
## ---- Local
# /lib/systemd/system/avahi-daemon.service
# This file is part of avahi.
## ----- Notamos que na linha 
# [Unit]
# Description=Avahi mDNS/DNS-SD Stack
# Requires=avahi-daemon.socket
## ----- Vemos que o avahi-daemon.service depede do socket para rodar
## ----- Porém executamos o mesmo comando systemctl cat avahi-daemon.socket e sabemos que mesmo para
## ----- o socket funcionar, ele precisa do service estar rodando....

## systemctl -t service. Verifica todos os serviços em execução:
systemctl -t service

## systemctl halt. Desliga a máquina
systemctl halt

## systemctl reboot. Reinicializa a máquina
systemctl reboot

## journalctl -f. Verifica os logs
journalctl -f

## journalctl --since=today. Para ver logs do dia
journalctl --since=today

## systemctl kill servico. Para matar um serviço.
# Executamdp systemctl poderá ver o PID dos processos
systemctl kill numero_PID 

## hostnamectl. Mostra informações da máquina.
hostnamectl

## timedatectl. Mostra data, hora e Timezone.
timedatectl

## ---------------------------

# O sistemd ainda tem uma camada de compatibilidade com system-V, Encontramos ele em /etc/init.d
ls /etc/init.d

## ls /etc/systemd/system/ - onde localiza o systemd
ls /etc/systemd/
# Vemos que mostra arquivos e diretórios
## O principal em que vamos falar agora é diretorio "system"
ls -l /etc/systemd/system
# E vemos que contém alguns links simbólicos para /lib/systemd/system/

## Comparando os arquivos de cada init system, vemos que é um shell scripting
cat -n /etc/init.d/bluetooth | less
# ---- são 134 linhas
# Mas a grande sacada do systemd é que ele usa uma quantidade bem menor de scripts do que system-V
# Podemos ver isso com os comandos
systemctl cat bluetooth.service
systemctl cat bluetooth.service | wc -l
# ---- são apenas 21 linhas
# Sendo assim, fez com que as empresas adotassem o systemd e começassem a dar suporte. Elas não precisariam ter
# um desenvolvedor só para fazer varias linhas de scripts, por exemplo para fazer o bluetooth funcionar
