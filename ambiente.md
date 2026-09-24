# Ambiente de trabalho

## Tipo de ambiente
VM local (WSL2 - Windows Subsystem for Linux, kernel Linux real integrado ao Windows)

## Sistema utilizado
Ubuntu 22.04.3 LTS (Jammy Jellyfish), kernel 6.6.87.1-microsoft-standard-WSL2, arquitetura x86_64

## Recursos disponíveis
CPU: 8 núcleos lógicos
RAM: 7.6 GiB total (6.9 GiB livres em repouso)
Disco: 1007 GiB no sistema de ficheiros raiz (2.2 GiB usados, 954 GiB disponíveis)

## Limitações
Os recursos são partilhados dinamicamente com o Windows anfitrião (não há reserva fixa de
CPU/RAM só para o WSL2). Não há IP público nem exposição direta à internet, ao contrário
de uma VPS — para os tópicos de rede, acesso remoto por SSH externo e serviço web exposto
publicamente, será necessário complementar com uma VPS real (ex.: Hostinger, referida nas
ferramentas do módulo) ou um ambiente Linux no browser.

## Observações
Este ambiente foi escolhido por já estar disponível na máquina, sem necessidade de
instalação de VirtualBox nem de criação de conta cloud, e por oferecer um terminal Linux
completo e persistente entre sessões (as pastas e ficheiros criados mantêm-se de sessão
para sessão). Será usado como ambiente principal de prática ao longo do módulo para todos
os tópicos que não exijam explicitamente uma VPS com IP público (ex.: SSH remoto real,
exposição de um serviço web na internet), altura em que se avaliará a criação de uma VPS.
