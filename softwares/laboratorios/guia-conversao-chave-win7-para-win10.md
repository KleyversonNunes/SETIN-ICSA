# Guia Técnico: Conversão de Chaves Físicas Windows 7 Pro (COA) em Licenças Digitais Legítimas do Windows 10 Pro

Este guia documenta o método avançado de bancada para ativação e conversão de chaves físicas de licenças legadas (adesivos COA de Windows 7 Pro) em Direito Digital permanentemente atrelado ao hardware (`HWID`) no Windows 10 Pro.

Este procedimento é ideal para ambientes de laboratório com hardware antigo que não possui a tabela `MSDM` injetada nativamente na BIOS (firmwares da era pré-Windows 8), contornando o bloqueio padrão da interface gráfica da Microsoft.

## 🔍 Visão Geral do Cenário e do Problema
Em computadores antigos (ex: placas-mãe com tabela `SLIC 2.1`), as chaves de 25 caracteres existem apenas no mundo físico (no adesivo colado no gabinete). Ao tentar inserir essa chave diretamente em uma instalação limpa do Windows 10 Pro através do menu clássico de configurações, os servidores de ativação frequentemente rejeitam o código, gerando erros de canal inválido.

O método documentado abaixo utiliza uma ferramenta de emulação de ticket para desentupir o canal de comunicação com o servidor da Microsoft, registrando o ID de hardware da máquina e permitindo a subsequente inserção e validação da chave física legítima da máquina, convertendo o canal para `OEM:DM`.

## 📋 Pré-requisitos
- Computador com gabinete contendo o adesivo físico (COA) legível do Windows 7 Professional.
- Instalação limpa do Windows 10 Pro já realizada na máquina.
- Conexão ativa com a Internet.
- Direitos de Administrador no sistema operacional.

## 🛠️ Passo a Passo da Operação
### Passo 1: Preparação do Sistema
Certifique-se de que a máquina está conectada à internet. O Windows subirá inicialmente sem ativação ou com um status de erro de licença devido à imagem clonada ou chave genérica de instalação.

### Passo 2: O Gatilho do Canal
Para forçar o servidor da Microsoft a gerar um registro de Hardware ID para esta placa-mãe, utilizaremos o mecanismo de emulação de ticket de migração.

1. Abra o **PowerShell** como **Administrador**.
2. Execute o comando oficial do Microsoft Activation Scripts (MAS) para iniciar a ferramenta em modo interativo:
```PowerShell
irm https://get.massgrave.dev | iex
```
3. No menu numérico que surgirá na tela, selecione a **Opção 1 (HWID)**.
4. O script enviará um token de emulação de upgrade para os servidores oficiais da Microsoft (`licensing.mp.microsoft.com`*).
5. Aguarde a mensagem de sucesso. Neste momento, a máquina recebeu temporariamente uma licença digital baseada em chave genérica global do canal `RETAIL`.

### Passo 3: Vinculação da Chave Física Legítima
Agora que o servidor da Microsoft aceitou a assinatura de hardware desta máquina, a interface gráfica do Windows permitirá a substituição da chave genérica pela chave real do gabinete.

1. No Windows 10, navegue até: **Configurações > Atualização e Segurança > Ativação**.
2. Clique em Alterar chave do produto.
3. Digite pausadamente os 25 caracteres da chave contida no **adesivo físico do gabinete (Windows 7 Pro)** e avance.
4. O Windows enviará o código real ao servidor. Como o canal de hardware já possui um Direito Digital ativo, o servidor validará o código físico do adesivo e fará a substituição imediata do canal de licenciamento.

## 📊 Auditoria e Validação do Status Legal
Para garantir que o processo foi concluído com sucesso e que a máquina está operando sob uma licença estritamente legítima e original, execute a auditoria via terminal.

1. Abra o **Prompt de Comando (CMD)** como **Administrador**.
2. Execute o comando de diagnóstico de licenciamento:
```DOS
slmgr /dlv
```
3. Avalie a janela de pop-up gerada pelo sistema. Para o sucesso do procedimento, os campos devem reportar exatamente os seguintes dados:

| **Campo na Janela** `slmgr` | Status Esperado | Siginificado Técnico |
| --- | --- | --- |
| **Descrição** | Operating System, Windows(R) Operating System, `RETAIL` channel | Indica que a máquinaherdou o direito de ativação digital |
| **Canal da Chave do Produto** | `OEM:DM` | **Confirmação de Sucesso**. Achave física do adesivo substitui o script e foi assinada como marcador digital original da fabricante. |
| **Status da Licença** | Licenciado | O sistema está operando de forma 100% legal e permanente |

1. Para confirmar que a licença é vitalícia, execute:
```DOS
slmgr /xpr
```
📢 O retorno deve ser explicitamente: `"O computador está ativado permanentemente"`.

## 🧠 Como Isso Funciona Debaixo do Capô?
```text
[Máquina Sem Licença] 
        │
        ▼
[Execução do MAS (HWID)] ────► Envia token de emulação ao servidor Microsoft.
        │
        ▼
[Servidor Carimba a RAM] ────► Registra a Placa-Mãe na nuvem sob o canal RETAIL temporário.
        │
        ▼
[Injeção da Chave Física] ───► Substitui a chave temporária pelo código real do adesivo.
        │
        ▼
[Status Final: OEM:DM]   ────► Licença convertida em Direito Digital Vitalício na Nuvem.
```

O segredo deste método está no comportamento do ecossistema de ativação da Microsoft. A interface de configurações rejeita a chave do Windows 7 de forma direta porque tenta fazer uma validação "fria".
Ao utilizar o script em modo HWID primeiro, o canal de comunicação é aberto e a assinatura física da placa-mãe é registrada na nuvem. Quando você injeta a chave do adesivo logo em seguida, o servidor aceita a substituição porque reconhece que o hardware já está autorizado a possuir o Windows 10 Pro. O resultado final é um computador **perfeitamente legalizado, auditável e com canal original de fábrica** (`OEM:DM`).

**💡 Nota de Sysadmin**: Uma vez concluído este processo, o Direito Digital deste computador está registrado de forma eterna nos servidores da Microsoft. Se este laboratório for formatado novamente no futuro (via Clonezilla, Ansible ou mídia limpa), não haverá necessidade de executar este guia ou digitar chaves novamente. Basta conectar o PC à internet que o Windows 10 Pro ativará de forma automática em segundos.
