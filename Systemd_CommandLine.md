
# Lista de comandos úteis do Systemd no dia a dia do administrador de sistemas Linux:


### Veja o manual, caso queira saber algo a mais

man systemd

### Mostra todo o tempo de  processo de inicialização do boot, que o cuida o kernel.

systemd-analyze

### Mostra mais detalhes da inicialização.

systemd-analyze blame

### Comando semelhante ao, comando top.

systemd-cgtop

### para saber algum comando a mais.

systemd-cgtop --help

### Comando para ver o status de um serviço, a fins de gerencia-lo.

systemctl status ssh

### Comando para inicializar um serviço.

systemctl start ssh

### Comando para parar um serviço.

systemctl stop ssh

### Comando para reinicializar um serviço.

systemctl restart ssh

### Comando para desabilitar um serviço. obs: Executa-lo como root

systemctl disable avahi-daemon.service avahi-daemon.socket

Isso só ocorrerá no proximo boot,
então é recomendado para-lo primeiro com o comando systemctl stop nome

### Comando para habilitar um serviço.

systemctl enable avahi-daemon.service avahi-daemon.socket

### Comando para ver se o serviço está habilitado.

systemctl is-enabled avahi-daemon.service

Caso queira ver o arquivo, a fins de saber sobre a diferença de service e socket

systemctl cat avahi-daemon.service

/lib/systemd/system/avahi-daemon.service
This file is part of avahi.
Notamos que na linha
 [Unit]
 Description=Avahi mDNS/DNS-SD Stack
 Requires=avahi-daemon.socket
----- Vemos que o avahi-daemon.service depede do socket para rodar
----- Porém executamos o mesmo comando systemctl cat avahi-daemon.socket e sabemos que mesmo para
----- o socket funcionar, ele precisa do service estar rodando....

### Comando para verificar todos os serviços em execução:

systemctl -t service

### Desligar a máquina.

systemctl halt

### Reinicializar a máquina.

systemctl reboot

### Verificar logs.

journalctl -f

# Comando para ver logs do dia.

journalctl --since=today

comando para matar um serviço.
Executando systemctl poderá ver o PID dos processos

systemctl kill numero_PID 

### Mostrar informações da máquina.

hostnamectl

### Mostrar data, hora e Timezone.

timedatectl


### O sistemd ainda tem uma camada de compatibilidade com system-V, Encontramos ele em /etc/init.d

ls /etc/init.d

ls /etc/systemd/system/ <- onde localiza o systemd

### mostrar arquivos e diretórios.

ls /etc/systemd/

### Vamos para o diretório "system"

ls -l /etc/systemd/system

Vemos que contém alguns links simbólicos para /lib/systemd/system/

Comparando os arquivos de cada init system, vemos que é um shell scripting

cat -n /etc/init.d/bluetooth | less
---- são 134 linhas
Mas a grande sacada do systemd é que ele usa uma quantidade bem menor de scripts do que system-V
Podemos ver isso com os comandos

systemctl cat bluetooth.service
systemctl cat bluetooth.service | wc -l
---- são apenas 21 linhas

Sendo assim, fez com que as empresas adotassem o systemd e começassem a dar suporte. Elas não precisariam ter
um desenvolvedor só para fazer varias linhas de scripts, por exemplo para fazer o bluetooth funcionar
