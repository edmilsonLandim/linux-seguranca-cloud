# Tópico 1 - Preparação do ambiente Linux

## Ambiente utilizado
WSL2 (Windows Subsystem for Linux, versão 2) com a distribuição Ubuntu 22.04.3 LTS.
O WSL2 corre um kernel Linux real (`6.6.87.1-microsoft-standard-WSL2`) dentro de uma máquina
virtual leve gerida pelo Hyper-V, integrada ao Windows. Por dar acesso a um terminal Linux
completo e persistente (equivalente, para efeitos desta atividade, a uma VM local), foi seguido
o **Trilho A**. É uma das ferramentas sugeridas na secção "Ferramentas e Software" do módulo no
Moodle, a par do VirtualBox/Ubuntu Server.

Utilizador: `edmilson`
Máquina (hostname): `RAI-GV-NOSI-189`

## Objetivo desta atividade
Preparar o ambiente Linux e organizar a primeira evidência técnica.

## Estrutura criada
```
linux-seguranca-cloud/
├── topico-01/
│   ├── evidencias/
│   │   ├── terminal-output.txt   (transcript dos comandos iniciais e respetivos outputs)
│   │   └── estrutura-criada.txt  (output do comando find, confirmando a árvore de pastas)
│   ├── comandos-topico-01.txt
│   └── README.md
└── produto-final/
```
A estrutura foi criada com um único comando `mkdir -p`, que cria todos os diretórios
intermédios necessários numa só instrução.

## Comandos executados
- `whoami` — identificar o utilizador atual.
- `hostname` — identificar o nome da máquina.
- `pwd` — confirmar o diretório de trabalho atual.
- `ls -l ~` — listar o conteúdo do diretório pessoal em detalhe.
- `df -h /` — verificar o espaço em disco disponível.
- `free -h` — verificar a memória RAM e swap disponíveis.
- `uname -a` — confirmar a versão do kernel e a arquitetura do sistema.
- `lsb_release -a` — confirmar a distribuição e versão do Linux instalado.
- `mkdir -p ...` — criar a estrutura de diretórios do projeto.
- `find linux-seguranca-cloud | sort` — listar recursivamente a estrutura criada, como evidência.

(Lista completa, com finalidade detalhada de cada comando, em `comandos-topico-01.txt`.)

## Diferença entre VM, VPS e infraestrutura em nuvem
- **VM (máquina virtual) local**: sistema operativo virtualizado que corre dentro do próprio
  computador do utilizador (ex.: VirtualBox, ou o WSL2 usado aqui), sobre o hardware local.
  Sem custos recorrentes, ideal para praticar, errar e recriar o ambiente à vontade, mas
  depende dos recursos (CPU/RAM/disco) da máquina física e não é acessível pela internet.
- **VPS (servidor virtual privado)**: servidor virtual que corre num datacenter remoto,
  com IP público, sistema operativo próprio e acesso via internet (normalmente por SSH).
  Aproxima-se mais da operação real de um servidor em produção, mas normalmente implica
  criar uma conta e pagar pelos recursos utilizados.
- **Infraestrutura em nuvem (cloud)**: conjunto mais amplo de serviços geridos por um
  fornecedor (ex.: AWS, Azure) — não é só um servidor, mas também rede, armazenamento,
  bases de dados, identidades e políticas de acesso, escaláveis e faturados por utilização.
  Uma VPS pode ser apenas um dos recursos disponibilizados dentro dessa infraestrutura.

Neste módulo, a VM/WSL local serve para praticar sem custos e sem risco; mais à frente,
os mesmos conceitos e comandos serão aplicados a uma VPS real, onde entram também
responsabilidades de segurança de rede e de acesso que não existem (ou são geridas de
forma diferente) num ambiente puramente local.

## Dificuldades encontradas
Nenhuma dificuldade técnica relevante. Não foi utilizada uma VM em VirtualBox porque o
WSL2 já disponibiliza um kernel Linux real e um terminal totalmente funcional na máquina
local, servindo o mesmo propósito para esta primeira atividade.

## Próximos passos
Para o próximo tópico (Gestão de Identidades e Acessos), é necessário ter este ambiente
Linux já preparado e acessível, para configurar utilizadores, grupos, permissões e,
mais adiante, o acesso remoto seguro por SSH (incluindo, eventualmente, migrar parte da
prática para uma VPS real, de forma a testar também o acesso remoto pela rede).
