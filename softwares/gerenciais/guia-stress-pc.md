# 🛠️ Procedimento de Validação de Hardware e Estabilidade (Linux)
Este documento descreve a rotina de testes de bancada utilizando distribuições Linux (instaladas ou via Live USB) para homologação de hardware e diagnóstico de instabilidades. O objetivo é isolar falhas físicas através de testes de estresse documentados e empíricos.

## 📦 1. Preparação do Ambiente
Antes de iniciar as validações, é necessário instalar as ferramentas de monitoramento e estresse. Em distribuições baseadas em Debian/Ubuntu (como o Linux Mint), utilize o comando abaixo:

```bash
sudo apt update
sudo apt install s-tui stress stress-ng lm-sensors smartmontools nvme-cli
```

## 🔍 2. Triagem de Baixo Nível (Logs do Kernel)
Antes de forçar o equipamento, devemos verificar se o próprio kernel do sistema operacional já identificou falhas de comunicação com os componentes físicos.

Comando:

```bash
sudo dmesg | grep -iE 'error|fail|critical|mce|hardware'
```
**O que faz:** Filtra as mensagens de inicialização do kernel (dmesg) em busca de termos críticos relacionados a falhas de hardware estruturais.

**Como analisar o resultado:**
- **Normal:** Mensagens sobre ACPI AE_ERROR (controle de energia do barramento) ou falhas no TPM são ruídos comuns e não afetam a estabilidade.
- **Falha Crítica:** Linhas contendo MCE (Machine Check Exception) indicam falhas físicas no processador. Mensagens contendo blk_update_request: I/O error indicam falha grave na controladora de disco ou barramento PCIe.

## 🌡️ 3. Monitoramento Térmico e Estresse Básico (s-tui)
O s-tui oferece uma interface gráfica no próprio terminal para visualizar em tempo real o comportamento térmico e elétrico do processador.

**Comando:**
```bash
sudo s-tui
```
**O que faz:** Gera um painel interativo exibindo Temperaturas, Frequência (MHz), Utilização (%) e Consumo (Watts). Pode ser integrado à ferramenta stress (selecionando a opção "Stress" na barra lateral) para gerar carga contínua na CPU.

**Como analisar o resultado:**

**Frequência:** O gráfico deve formar uma "parede" reta, mantendo o clock no máximo (Turbo) de forma estável. Quedas abruptas formam "vales" no gráfico e indicam engasgos no fornecimento de energia (VRM da placa-mãe).

**Temperatura:** Não deve ultrapassar o limite crítico do processador (geralmente entre 95°C e 105°C). Se atingir esse teto e a frequência cair, o sistema está sofrendo Thermal Throttling (deficiência na refrigeração).

**Fonte de Alimentação:** Se o computador desligar abruptamente (tela preta instantânea) ao iniciar o estresse, a fonte de alimentação não suporta o pico de carga.

## 🌪️ 4. Teste Caótico Multivariável (stress-ng)
Este é o teste definitivo de estabilidade. Ele gera cargas violentas, oscilantes e simultâneas em vários componentes do sistema, simulando cenários extremos.

**Comando:**

```bash
sudo stress-ng --cpu 0 --matrix 0 --hdd 1 --vm 1 --vm-bytes 85% --timeout 60m --metrics-brief
```
**O que os parâmetros fazem:**
- --cpu 0: Usa todos os núcleos lógicos disponíveis para operações gerais.
- --matrix 0: Força todos os núcleos a calcularem operações matemáticas complexas com matrizes.
- --hdd 1: Cria um processo trabalhador gerando ciclos ininterruptos de leitura e gravação no disco.
- --vm 1 --vm-bytes 85%: Cria um processo para alocar estritamente 85% de toda a memória RAM livre do sistema, forçando falhas de comunicação sem acionar o limitador do sistema (OOM Killer).
- --timeout 60m: Define a duração do teste (neste caso, 1 hora).

**Como analisar o resultado:**

**Falha (Failed > 0):** Se o terminal relatar qualquer falha nos stressors, se o processo for interrompido por Segmentation Fault, ou se o computador congelar/reiniciar, o hardware base está instável (Fonte, RAM, CPU ou VRM).

**Sucesso:** A mensagem final deve ser successful run completed in X hours, Y secs e failed: 0. Isso garante integridade absoluta na entrega de energia e processamento.

## 💽 5. Avaliação Lógica e Física de Armazenamento
Testes para garantir que o HD mecânico ou SSD não apresenta desgaste físico ou gargalos lógicos.

### 5.1. HDs e SSDs SATA (Via Interface Gráfica)
**Ferramenta:** GNOME Disks (Utilitário de Discos).

**O que faz e como analisar:**
- **Acessar Dados e Testes S.M.A.R.T.:** A Avaliação Geral deve indicar "O disco está OK". Parâmetros como Setores Realocados ou Setores Pendentes devem estar zerados.

- **Executar Avaliação de Desempenho:** Em HDs mecânicos, a taxa de leitura deve iniciar alta e cair gradativamente, sem interrupções abruptas ou quedas para 0 MB/s. Em SSDs, a linha de leitura deve ser reta e constante, com tempo de acesso baixíssimo (frações de milissegundo).

### 5.2. SSDs M.2 NVMe (Via Terminal)
**Comando:**

```bash
sudo nvme error-log /dev/nvme0
```
**O que faz:** Acessa os logs nativos do protocolo NVMe no barramento PCIe.

**Como analisar o resultado:** Se o log listar diversos erros de "aborto de comandos", a controladora do SSD NVMe está perdendo comunicação com a placa-mãe. Durante o teste do stress-ng, monitore via s-tui (ou HWiNFO em Windows) se a controladora do NVMe ultrapassa os 75°C, o que causa estrangulamento térmico (sendo necessária a aplicação de thermal pads/dissipadores).

## 🧠 6. Teste Profundo de Memória (Memtest86+)
Embora o parâmetro --vm do stress-ng valide a estabilidade lógica da RAM, a homologação final para descartar micro-corrupções físicas em setores isolados exige um teste fora do sistema operacional.

- **Procedimento:** Realizar o boot via pendrive (Ventoy) com a imagem .iso do Memtest86+.
- **Execução recomendada:** Devido à longa duração matemática do teste, recomenda-se iniciar no fim do expediente.
- **Análise:** O hardware é considerado íntegro se o sistema completar no mínimo 1 ciclo inteiro (Pass: 1) sem registrar nenhuma linha de erro em vermelho na tela.
