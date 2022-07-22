### 2.1 - Gerando sua chave pública e privada do servidor

Nesta etapa, vamos criar uma uma chave de criptografia GPG associada ao seu e-mail para criptografar os backups salvos na nuvem. 

- [ ] Execute os comandos no terminal. 
  ```
  sudo su
  gpg --full-generate-key --expert
  ○ Please select what kind of key you want: Your Selection? 1
  ○ What keysize do you want? 4096
  ○ What keysize do you want for the subkey? 4096
  ○ Key is valid for? 0 
  ○ Is this correct? Y
  ○ Real Name: Primeiro e Último nome
  ○ Email Address: emaildoexperimento@protonmail.com
  ○ Comment: Nome do Seu Experimento
  ○ Change (N)ame, (C)omment (E)mail (O)kay/(Q)uit? O
  ○ Please enter the passphrase to protect your new key: 
  Crie uma senha forte e anote para salvar no seu gestor de senhas após concluir o procedimento
  ```

- [ ] Localize a assinatura que gerou executando o comando abaixo, 
  ```
  gpg -k user@email
  ```
    
- [ ] Anote o ID da assinatura que deve se parecer com esse 2050D19261ADE8E9493FE78F793617911886BBC5
  ```
  gpg --output public.pgp --armor --export ID
  gpg --output private.pgp --armor --export-secret-key ID
  gpg --output backupkeys.pgp --armor --export-secret-keys --export-options export-backup ID
  ```

- [ ] Faça um backup dos arquivos das chaves public.pgp, private.pgp e  backupkeys.pgp no gerenciador de senhas:  
  	- [ ] Faça Login na sua conta do Gerenciador de Senhas de sua preferência e crie uma identidade 
  	- [ ] Se estiver usando uma conta paga poderá salvar os arquivos, se não, copie o conteúdo dos arquivos em notas. 
  	- [ ] Fazer uma Cópia Offline e guarde-a com segurança. Converta as chaves com o Paperkey https://github.com/dmshaw/paperkey/ 



#

### 2.2 - Instalação script de Backup
Instale o script de Backup criado para fazer backups regulares e enviar para o google drive
  
  - [ ] Rode os comandos no terminal
	```
	sudo su
	cd /home/admin/
	git clone https://github.com/PraiaBitcoin/MyNodeBTC-Backup
	cd MyNodeBTC-Backup
	gpg -k seuemail@protonmail.com
	```
  - [ ] Copie seu ID e altere o arquivo  
	```
	nano .config 
	**PUBLIKEY=”seu_id_entre_aspas”
	PASSWORD=Sua_Senha_da__Assinatura_GPG**
	```
	- [ ] No arquivo já consta o ID da Bitcoin Beach Brazil. Substitua pelo
	seu ID.
	- [ ] Em Password “Insira a senha da sua chave privada”. 

  - [ ] Após alterar o arquivo, salve e execute os últimos dois comandos
	```
	chmod 755 install.sh
	sudo ./install.sh
	```


#### Lista de Comandos do Serviço de Backup
Comando para verificar o status do serviço
```
sudo systemctl status backupmynode.service
```

Comando para verificar o status do timer
```
sudo systemctl status backupmynode.timer
```

Comando para executar um backup manualmente 
```
sudo /home/admin/scripts/MyNodeBTC-Backup/backupmynode.sh
```

Verifique se o arquivo foi criptografado
```
sudo su
cd /mnt/hdd/BACKUP
gpg -d nomedoarquivo.tar.asc > nomedearquivosdesaida.tar
```

Verifique os arquivos criados descompactando-os
```
tar tfv /mnt/hdd/BACKUP/nomedearquivosdesaida.tar
```

Verifique os arquivos criados no Google Drive

#
