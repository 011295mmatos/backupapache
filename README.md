# backupapache
Backup Server Apache


Este script tem como ação realizar o backup de um servidor Apache, mais especificadamente os diretórios default e seu conteúdo.


Para gerar a automatização do script de backup sendo ele realizado em todas as madrugadas favor inserir a seguinte linha no cron utilizando o crontab -e

00 02 * * * inserir o caminho onde o script está salvo

Exemplo

00 02 * * * /home/matheus/documentos/backup.sh

#!/bin/bash

echo "VALIDANDO DIRETÓRIO DE BACKUP"
if [ -d "/home/backup_apache" ]
then
	echo "Diretorio de backup existente"
else
	echo "Diretorio não existente"
	mkdir /home/backup_apache
	echo "Diretorio de backup criado"
fi

echo "***SCRIPT BACKUP APACHE SERVER***"
echo "Este script tem como atuação realizar o backup de arquivos de log e conteúdo de um apache server."
echo "Seguir as instruções abaixo:"
date

sleep 2

echo "Digite a opção de backup a ser feita:"

sleep 2

echo "1 - Logs do diretório default"
echo "2 - Logs do diretório responsável pelo conteúdo do servidor"
echo "3 - Logs do diretório default e conteúdo do servidor"
read valor

if [ $valor == 1 ]
then
	#Declarando variáveis com diretorios para backup
	backup_arquivos="/etc/apache2"
	destino="/home/backup_apache"

	#Declarando variaveis para geração de arquivo
	dia=$(date +%d-%m-%y)
	hostname=$(hostname -s)
	arquivo="diretorio_default_$hostname-$dia.tar.gz"

	sleep 2

	echo "Realizando backup das pastas $backup_arquivos para $destino/$arquivo"

	sleep 2

	tar -zcvf $destino/$arquivo $backup_arquivos

	sleep 3

	echo "Backup realizado com sucesso"

	ls -lh $destino
fi

if [ $valor == 2 ]
then
	#Declarando variáveis com diretorios para backup
	backup_arquivos="/var/www*"
	destino="/home/backup_apache"

	#Declarando variaveis para geração de arquivo
	dia=$(date +%d-%m-%y)
	hostname=$(hostname -s)
	arquivo="conteudo_servidor_$hostname-$dia.tar.gz"

	sleep 2

	echo "Realizando backup das pastas $backup_arquivos para $destino/$arquivo"

	sleep 2

	tar -zcvf $destino/$arquivo $backup_arquivos

	sleep 3

	echo "Backup realizado com sucesso"

	ls -lh $destino
fi

if [ $valor == 3 ]
then
	#Declarando variáveis com diretorios para backup
	backup_arquivos="/etc/apache2 /var/www*"
	destino="/home/backup_apache"

	#Declarando variaveis para geração de arquivo
	dia=$(date +%d-%m-%y)
	hostname=$(hostname -s)
	arquivo="backup_completo_$hostname-$dia.tar.gz"

	sleep 2

	echo "Realizando backup das pastas $backup_arquivos para $destino/$arquivo"

	sleep 2

	tar -zcvf $destino/$arquivo $backup_arquivos

	sleep 3

	echo "Backup realizado com sucesso"

	ls -lh $destino
fi


if [ $valor != 1 ] || [ $valor != 2 ] || [ $valor != 3 ]
then
	echo "Opção inválida. Favor digitar a opção correta."
fi

