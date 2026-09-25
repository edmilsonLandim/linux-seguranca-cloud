# Firewall (UFW)

## Estado inicial
```
$ sudo ufw status verbose
Status: inactive
```
O UFW já estava instalado nesta instância (pacote `ufw 0.36.1-4ubuntu0.1`), mas
sem nenhuma regra ativa e sem política aplicada.

## Regras aplicadas
```
$ sudo ufw allow 80/tcp
Rules updated
Rules updated (v6)

$ sudo ufw enable
Firewall is active and enabled on system startup
```

**SSH:** não foi criada nenhuma regra para SSH porque não existe servidor SSH
instalado nesta instância (ver Tópico 2 - `openssh-server` ausente). Se o SSH
vier a ser instalado num tópico futuro, a regra de permissão **tem de** ser
criada e confirmada *antes* de ativar (ou reativar) o UFW, exatamente pelo
aviso da Secção 5 do enunciado: ativar o firewall sem garantir acesso SSH pode
bloquear o próprio acesso remoto ao servidor. Neste ambiente local (WSL2) esse
risco não se aplica da mesma forma, porque o acesso à instância não depende da
rede (é feito diretamente pelo Windows via `wsl.exe`), mas seria crítico numa
VPS real.

**HTTPS (443):** não foi aberta, porque o Nginx não tem nenhum certificado TLS
configurado - abrir a porta sem o serviço correspondente aumentaria a
superfície de ataque sem qualquer benefício.

## Estado final
```
$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp                     ALLOW IN    Anywhere
80/tcp (v6)                ALLOW IN    Anywhere (v6)
```
Política aplicada: **negar todo o tráfego de entrada por omissão**, permitindo
apenas a porta 80/tcp (IPv4 e IPv6) - exatamente o mínimo necessário para o
serviço web do Tópico 3 continuar acessível, e nada mais.

Evidência completa em `evidencias/firewall-depois.txt`.
