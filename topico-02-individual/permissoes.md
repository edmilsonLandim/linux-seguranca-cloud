# Permissões aplicadas

## Ambiente utilizado
VM local (WSL2 - Ubuntu 22.04.3 LTS), o mesmo ambiente já utilizado e documentado no
Tópico 1 (`linux-seguranca-cloud/topico-01/README.md`).

## Utilizador e grupos
```
$ whoami
edmilson

$ id
uid=1000(edmilson) gid=1000(edmilson) groups=1000(edmilson),4(adm),20(dialout),
24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),116(netdev)

$ groups
edmilson adm dialout cdrom floppy sudo audio dip video plugdev netdev
```
Resumo: o utilizador `edmilson` tem UID 1000 (utilizador comum, não root) e pertence a
11 grupos secundários. O mais relevante para a gestão de privilégios é o grupo `sudo`,
que lhe permite executar comandos como administrador *através de autenticação explícita*
(password), e não por defeito — o que já é, em si, uma aplicação do princípio do menor
privilégio: mesmo pertencendo ao grupo `sudo`, o utilizador não opera como root em
permanência.

## Ficheiros criados
- **publico.txt**: ficheiro de teste com conteúdo não sensível, pensado para poder ser
  lido por qualquer utilizador do sistema.
- **restrito.txt**: ficheiro de teste que simula conteúdo mais sensível, pensado para
  ser lido apenas pelo dono e pelo grupo de trabalho, nunca por "outros".
- **script.sh**: pequeno script de shell que imprime uma mensagem de confirmação,
  usado para testar a permissão de execução.

## Permissões aplicadas

| Ficheiro | Permissão | Justificação |
|---|---:|---|
| publico.txt | 644 (rw-r--r--) | Conteúdo não sensível: o dono precisa de ler e escrever, mas basta que o grupo e os restantes utilizadores consigam apenas ler — não há necessidade de lhes dar escrita nem execução. |
| restrito.txt | 640 (rw-r-----) | Conteúdo mais sensível: mantém leitura/escrita para o dono e leitura para o grupo (colegas de trabalho), mas remove qualquer acesso a "outros", reduzindo a exposição da informação a quem não precisa dela. |
| script.sh | u+x (acrescentado a rw-r--r--, resultando em rwxr--r--) | Só o dono precisa de executar o script nesta fase de testes; o grupo e outros mantêm apenas leitura (podem ver o conteúdo do script, mas não corrê-lo nem alterá-lo), evitando execução não controlada por terceiros. |

Evidência do estado antes e depois em
`evidencias/permissoes-antes.txt` e `evidencias/permissoes-depois.txt`, incluindo o
output de `./script.sh` a confirmar a execução bem-sucedida.

## Relação com o princípio do menor privilégio
Em nenhum dos três casos foi usada uma permissão "total para todos" (ex.: `chmod 777`
ou `chmod 666`), que daria a qualquer utilizador do sistema a capacidade de ler,
escrever ou executar ficheiros sem necessidade real disso. Em vez disso, cada
permissão foi ajustada ao mínimo necessário para cada perfil de acesso:

- **publico.txt** só precisa de ser lido por terceiros, por isso não lhes é dado
  escrita nem execução.
- **restrito.txt** não deve ser sequer lido por quem não pertence ao grupo, por isso
  "outros" ficam sem qualquer acesso (o último dígito é `0`).
- **script.sh** só deve ser executado pelo dono nesta fase, por isso a permissão de
  execução foi adicionada apenas ao dono (`u+x`), e não ao grupo nem a outros.

Este raciocínio é exatamente o princípio do menor privilégio aplicado ao nível do
sistema de ficheiros: cada identidade (dono, grupo, outros) recebe apenas o acesso
estritamente necessário para a sua função, reduzindo a superfície de ataque em caso de
comprometimento de uma conta ou de erro humano.
