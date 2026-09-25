# Hardening (notas para o próximo nível)

*Esta atividade foi realizada ao Nível 2 (Intermédio - firewall). Este ficheiro
não é uma implementação completa de Nível 3, mas um levantamento das medidas de
hardening identificadas para o serviço Nginx publicado, a aplicar em tópicos
seguintes (Monitorização e Gestão de Vulnerabilidades).*

## Serviço em causa
Nginx 1.18.0 (Ubuntu), servindo conteúdo estático em `/var/www/html/topico-03/`.

## Riscos específicos deste serviço
- Cabeçalho `Server` do Nginx expõe a versão exata do software.
- Página de boas-vindas por omissão do Nginx (`/var/www/html/index.nginx-debian.html`)
  continua acessível, revelando que é um Nginx Ubuntu por omissão.
- Sem HTTPS: tráfego não cifrado.
- Sistema sem confirmação de atualização desde a instalação (Tópico 3).
- Sem ferramenta de deteção/reação a tentativas de acesso abusivas (o
  equivalente ao Fail2Ban só faz sentido quando existir SSH ou um formulário
  exposto - não há nenhum destes ainda).

## Medidas que podem ser aplicadas já (baixo risco, sem dependências)
- `sudo apt update && sudo apt upgrade` — aplicar correções de segurança
  pendentes ao sistema e ao próprio Nginx.
- Desativar o cabeçalho de versão do Nginx (`server_tokens off;` em
  `/etc/nginx/nginx.conf`) — reduz a informação exposta sem alterar
  funcionalidade.
- Remover ou substituir a página de boas-vindas por omissão do Nginx, para não
  sinalizar uma instalação "de fábrica" por corrigir.
- Confirmar que `/var/www/html/topico-03` mantém permissões `644` (ficheiros)
  e dono `www-data` — já verificado no Tópico 3, mas deve ser reconfirmado
  sempre que novos ficheiros forem adicionados.
- Rever `/etc/passwd` para confirmar que não existem contas de teste ou
  inativas nesta instância (neste momento só existe o utilizador `edmilson`).

## Medidas que ficam para tópicos seguintes (dependem de outros passos)
- **HTTPS/TLS:** requer domínio próprio ou certificado (ex.: Let's Encrypt),
  só fará sentido com uma VPS com IP público/domínio - fora do âmbito deste
  ambiente local.
- **Fail2Ban:** só é útil a partir do momento em que exista um serviço
  autenticado exposto (ex.: SSH, quando for instalado num tópico futuro) -
  instalar agora não traria benefício real.
- **SSH endurecido** (porta personalizada, sem login root, só por chave):
  aplicável apenas quando o `openssh-server` for instalado (ver limitação
  documentada no Tópico 2).
- **Auditoria com Lynis** (`sudo lynis audit system`): fica reservada para o
  Tópico 5 (Gestão de Vulnerabilidades e Riscos), onde a interpretação de
  verificações de segurança é o foco explícito do módulo.
- **Hardening de `/tmp`** (`noexec`, `nosuid`, `nodev` via `/etc/fstab`): não
  é trivial de replicar de forma fiável em WSL2 (o `/etc/fstab` não controla
  da mesma forma a montagem do sistema de ficheiros dentro do WSL) - fica
  documentado como medida a aplicar numa VPS real, não neste ambiente.

## Justificação da fronteira aplicado / adiado
As medidas "aplicadas já" foram escolhidas por serem de baixo risco, não
dependerem de infraestrutura adicional e não exigirem decisões que ainda não
foram tomadas no módulo (ex.: domínio, certificado). As medidas "adiadas"
dependem de peças que só existirão em tópicos posteriores (SSH, IP público,
foco explícito em auditoria) - implementá-las agora seria antecipar trabalho
sem o contexto necessário para o validar corretamente.
