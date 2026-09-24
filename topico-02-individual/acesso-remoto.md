# Acesso remoto por SSH

*(Opção B2 - Nível intermédio)*

## Estado do serviço SSH
```
$ systemctl status ssh
Unit ssh.service could not be found.

$ dpkg -l | grep openssh
ii  openssh-client   1:8.9p1-3ubuntu0.10   amd64   secure shell (SSH) client, for secure access to remote machines
```
Conclusão: neste ambiente (WSL2 - Ubuntu 22.04) está instalado apenas o **cliente** SSH
(`openssh-client`), que permite ligar a servidores remotos, mas **não** está instalado
o **servidor** SSH (`openssh-server`/`sshd`). Por isso não existe nenhum serviço `ssh`
para o `systemctl` gerir — daí o resultado "Unit ssh.service could not be found".

## Endereço identificado
```
$ hostname -I
172.22.182.65
```
Este é o endereço IP interno atribuído pelo WSL2 a esta instância, válido apenas dentro
da rede virtual criada pelo Windows para o WSL — não é um IP público nem acessível a
partir de outra máquina na internet.

## Comando de ligação
Caso o servidor SSH estivesse instalado e ativo nesta máquina, o comando de ligação a
partir de outro terminal (ex.: outra distribuição WSL, ou outra máquina na mesma rede)
seria:
```
ssh edmilson@172.22.182.65
```
ou, para indicar explicitamente a porta por omissão do SSH:
```
ssh -p 22 edmilson@172.22.182.65
```

## Resultado obtido
Não foi possível testar uma ligação SSH real porque:
1. O pacote `openssh-server` não está instalado nesta instância.
2. A instalação do pacote (`sudo apt install openssh-server`) exige autenticação por
   password através do `sudo`, que não está configurada para execução não interativa
   (`sudo -n true` devolveu `sudo: a password is required`).

Por este motivo, o teste foi documentado até ao ponto em que a falta de credenciais
interativas impede a continuação, em vez de ser simulado ou inventado.

## Limitações encontradas
- Ambiente sem servidor SSH instalado por omissão (comum em distribuições WSL, que
  privilegiam o acesso via terminal integrado do Windows e não o acesso remoto por
  rede).
- Instalação do `openssh-server` bloqueada, neste contexto, por exigir password de
  sudo interativa, que não é fornecida em automações não interativas — o que é, na
  prática, uma boa demonstração do princípio de proteção de credenciais: uma ação
  administrativa sensível (instalar e expor um serviço de acesso remoto) não deve
  ser possível sem autenticação explícita do utilizador.
- Mesmo que o servidor fosse instalado, o IP obtido (`172.22.182.65`) é interno ao
  WSL2 e não substitui o cenário de uma VPS real com IP público, que será o ambiente
  mais adequado para testar um acesso remoto por SSH "de fora para dentro" nos
  próximos tópicos do módulo.

## Próximo passo sugerido
Para uma demonstração completa de SSH, este exercício pode ser repetido:
- localmente, correndo `sudo apt install openssh-server` de forma interativa (fora
  desta automação, num terminal onde a password possa ser introduzida) e voltando a
  testar `systemctl status ssh` e `ssh edmilson@172.22.182.65`; ou
- numa VPS real (ex.: Hostinger, já referida nas ferramentas do módulo), onde o SSH
  já vem tipicamente instalado e ativo por omissão, com IP público.
