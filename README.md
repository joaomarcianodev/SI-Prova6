# SI-Prova6

# Apresentação

[Link da Apresentação](https://youtu.be/QlvAJISp-SI)

# Papéis de cada máquina 

## Vítima
Debian (NAT + Rede Interna)
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0 up

## Atacante
Kali (Rede Interna)
sudo ifconfig eth0 192.168.1.11 netmask 255.255.255.0 up

# Fase 1, ataque pelo SSH

## Novo usuário com senha fraca e Instalando ssh
(no Debian)
sudo adduser funcionario
(senha password)
(apertar ENTER pra tudo que vier depois)
sudo apt install openssh-server

## Ataque 
(No Kali)
nano senhas.txt
(escreva as linhas abaixo)
iftm
password
admin123
mudar123
123456
security
qwerty
(salvar o arquivo)
hydra -l funcionario -P senhas.txt ssh://192.168.1.10 -t 4

# Fase 2, 

## Fail2ban
(no Debian)
su -
apt update
apt install fail2ban -y
systemctl enable fail2ban
systemctl start fail2ban
fail2ban-client status sshd

## Autenticação de Dois Fatores
(no Debian)
su -
apt install libpam-google-authenticator -y
google-authenticator
(logar no celular e dar "y" para tudo)
exit

## Ativar MFA para acesso SSH
(no Debian)
nano /etc/pam.d/sshd
(na última linha do arquivo adicione:)
auth required pam_google_authenticator.so
(salvar arquivo)
nano /etc/ssh/sshd_config
(na frente de "KbdInteractiveAuthentication" troque "no" para "yes")
(salvar arquivo)
systemctl restart ssh
sudo reboot

## Teste na prática
(no Kali)
hydra -l funcionario -P senhas.txt ssh://192.168.1.10 -t 4
ssh funcionario@192.168.1.10
