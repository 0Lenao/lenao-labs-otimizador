# Lenão Labs Otimizador

Utilitário Windows (100% local, gratuito e sem API paga) que **avalia, entende e melhora**
o computador por três categorias — **desempenho**, **visual** e **economia de energia** —
levando em conta geração do processador, CPU, GPU, memória, armazenamento, tipo de PC
(desktop/notebook), velocidade da interface e latência/ping da internet.

Identidade visual baseada nas cores de **www.lenaolabs.com.br** (grafite-violeta e âmbar).

Site: https://www.lenaolabs.com.br

## Qual executável abrir?

| Arquivo | Para que serve |
|---|---|
| `dist\Instalador Lenão Labs Otimizador.exe` | **Baixe e abra este**: instala o programa, cria atalhos e o registro de desinstalação |
| `dist\Lenão Labs Otimizador.exe` | Programa principal (portátil; abre direto sem instalar) |
| `dist\Lenão Labs Otimizador-CLI.exe` | Linha de comando |

Para distribuir, envie a **pasta `dist` inteira** (inclui `PresentMon\`).

## Instalação

1. Abaixe a pasta `dist` na máquina de destino.
2. Execute `Instalador Lenão Labs Otimizador.exe`.
3. Clique em **Instalar**. O programa vai para
   `%LOCALAPPDATA%\LenaoLabs\Otimizador`, com atalhos na Área de Trabalho e no
   Menu Iniciar (além de um atalho para o site da Lenão Labs).
4. Para remover: use **Desinstalar** no instalador, ou *Adicionar ou remover
   programas* (o instalador cria a entrada no registro do Windows).

## O que ele faz

1. **Diagnóstico:** lê CPU (com geração), GPU (inclusive a dedicada quando há
   duas — ex.: GTX 1650 e Intel HD 610), RAM, discos (SSD/HD), placa-mãe, sistema,
   plano de energia, adaptadores de rede e DNS — via WMI/PowerShell.
2. **Rede:** mede latência (média, jitter e perda) com ICMP para Google, Cloudflare
   e o gateway. Não usa speedtest nem API paga.
3. **Classificação:** pontuação 0–100 e perfil (`Alto`, `Médio`, `Básico`, `Entrada`).
4. **Perfis por tipo de uso:** na aba **Programa / Jogo**, escolha Jogo,
   Escritório/produtividade, Navegador, Edição de vídeo/foto/3D, Programação,
   Bateria ou Uso geral — e opcionalmente selecione um processo para receber
   prioridade alta.
5. **Ajustes reversíveis:** cada valor original é salvo em `backups/estado.json`;
   **Reverter tudo** restaura na ordem inversa. Há botão para ponto de restauração.
6. **Painel na bandeja:** ícone nos *itens ocultos* da barra de tarefas com painel
   flutuante ao vivo com FPS, uso de GPU, temperaturas, RAM, disco, rede ↓/↑,
   processos, ping e tempo ligado. Clique no ícone para abrir/ocultar. O botão
   **Minimizar para bandeja** esconde a janela; **Sair** encerra de verdade.

### FPS e temperaturas

- A temperatura da **GPU NVIDIA** vem do NVML (drivers atuais).
- A temperatura da **CPU** depende do sensor exposto pelo Windows; se o hardware
  não expor (comum em desktops), o painel mostra `—`.
- O **FPS real** usa o [PresentMon](https://github.com/GameTechDev/PresentMon)
  (gratuito, incluído em `dist\PresentMon\`). O PresentMon exige administrador:
  use **Executar como administrador** no app para ativar o FPS. Sem isso, os
  demais dados continuam ao vivo e o Painel informa o motivo.

## Linha de comando

```
Lenão Labs Otimizador-CLI.exe diagnosticar
Lenão Labs Otimizador-CLI.exe diagnosticar --json
Lenão Labs Otimizador-CLI.exe cenarios
Lenão Labs Otimizador-CLI.exe processos --filtro chrome
Lenão Labs Otimizador-CLI.exe recomendados --cenario jogo
Lenão Labs Otimizador-CLI.exe aplicar --cenario jogo --programa msedge --dry-run
Lenão Labs Otimizador-CLI.exe aplicar --cenario criacao --pid 1234 --ponto
Lenão Labs Otimizador-CLI.exe reverter
Lenão Labs Otimizador-CLI.exe relatorio
Lenão Labs Otimizador-CLI.exe site
Lenão Labs Otimizador-CLI.exe admin
```

`--dry-run` simula e não altera nada. `--ponto` cria ponto de restauração.
Nunca é usada a classe de prioridade `Realtime`.

## Segurança

- Nenhum dado é enviado para a internet; não há telemetria própria nem APIs pagas.
- Comandos internos (PowerShell, PresentMon, registro) rodam **sem abrir janela**.
  Se o sensor de temperatura da CPU não existir, a consulta é feita uma única vez
  e desativada — sem janelas repetidas.
- Todos os valores anteriores são gravados antes da mudança.
- O boost de prioridade de processo é temporário (volta ao normal se o app parar
  ou ao usar **Reverter tudo**).
- Nunca mexe em senhas, documentos pessoais ou arquivos de inicialização.

## Recompilar

```
D:\Download\_ferramentas\pc-otimizador\venv\Scripts\python.exe build.py
```

Gera os três executáveis e copia `PresentMon\` para `dist\`.

## Limitações

- A pontuação é heurística: orienta, não é benchmark.
- Ping é latência ICMP; não mede vazão em Mbps (isso exigiria serviço externo).
- HAGS e prioridade de processos podem exigir reinício.
- FPS depende de PresentMon + administrador; CPU temp depende do sensor do Windows.
