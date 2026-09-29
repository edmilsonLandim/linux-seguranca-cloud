# Backup e recuperação

## Diretório crítico identificado
`/var/www/html/topico-03/` — conteúdo publicado do serviço web (Tópico 3).
Escolhido por ser o único diretório desta instância cuja perda teria impacto
direto e imediato num serviço em produção (o site deixaria de estar
disponível ou de mostrar o conteúdo correto).

## Criação do backup
```
$ tar -czf backups/backup-topico-03-20260929-103044.tar.gz -C /var/www/html topico-03

$ ls -l backups/
-rw-r--r-- 1 root root 1480 Sep 29 10:30 backup-topico-03-20260929-103044.tar.gz

$ tar -tzf backups/backup-topico-03-20260929-103044.tar.gz
topico-03/
topico-03/sobre.html
topico-03/style.css
topico-03/index.html
```
O nome do ficheiro inclui data e hora (`AAAAMMDD-HHMMSS`), permitindo manter
várias versões do backup sem se sobreporem.

## Pasta de teste
Criada em `teste-recuperacao/`, separada do diretório original e do próprio
backup - a recuperação nunca é testada diretamente sobre `/var/www/html`, para
não arriscar substituir dados válidos por um backup eventualmente corrompido
sem primeiro confirmar a sua integridade.

## Restauro do backup
```
$ tar -xzf backups/backup-topico-03-20260929-103044.tar.gz -C teste-recuperacao

$ ls -lR teste-recuperacao/
teste-recuperacao/topico-03:
-rw-r--r-- 1 www-data www-data 1360 Sep 24 12:44 index.html
-rw-r--r-- 1 www-data www-data 1093 Sep 24 12:44 sobre.html
-rw-r--r-- 1 www-data www-data  718 Sep 24 12:44 style.css
```
Note-se que o `tar` preservou o dono/grupo originais (`www-data`) e a data de
modificação original dos ficheiros (24/09, data da publicação no Tópico 3),
não a data do backup em si - confirmação adicional de que o conteúdo não foi
alterado no processo.

## Confirmação da recuperação
```
$ diff -r /var/www/html/topico-03 teste-recuperacao/topico-03
SEM DIFERENCAS - recuperacao identica ao original

$ md5sum /var/www/html/topico-03/* teste-recuperacao/topico-03/*
6c3a2669ab847619acfdf81254804bdf  /var/www/html/topico-03/index.html
5351f0ed9c3ab58f5883309b1f914655  /var/www/html/topico-03/sobre.html
704d2ef72c0a8edc829fb1a2c75affd2  /var/www/html/topico-03/style.css
6c3a2669ab847619acfdf81254804bdf  teste-recuperacao/topico-03/index.html
5351f0ed9c3ab58f5883309b1f914655  teste-recuperacao/topico-03/sobre.html
704d2ef72c0a8edc829fb1a2c75affd2  teste-recuperacao/topico-03/style.css
```
Os checksums MD5 dos três ficheiros restaurados são **idênticos** aos
originais - confirmação, ao nível de byte, de que o backup e a recuperação
funcionam corretamente, não apenas "parecem" funcionar por os nomes e tamanhos
coincidirem.

Evidências completas em `evidencias/backup-criado.txt` e
`evidencias/recuperacao-teste.txt`.
