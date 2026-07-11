# Guia - Politícas de Grupo no Windows 11 para PCs Administrativos
Este guia deve ser utilizado pela equipe do SETIN, como referência, para quais Políticas de Grupo configurar no Windows 11 Pro instalado nos computadores de uso administrativos do ICSA.

## Preparação do Arquivo `gpo_admin.msc`
O arquivo `gpo_admin.msc` será o meio que utilizado para configurar as políticas de grupo. Para criar esse arquivo siga a sequência abaixo:

1. Abra o executar do windows (`WIN+R`).
2. Digite `mmc` e pressione `Enter`.
3. Irá abrir uma janela do **Console de Gerenciamento Microsoft**, confirme sim.
4. Na nova janela clique no menu `Arquivo`, depois clique em `Adicionar/remover snap-in...`.
5. Na guia `Snap-ins disponíveis` selecione `Editor de Objeto de Política de Grupo`, depois clique em `Adicionar >`. Na janela que abrir, clique em `Procurar`, depois clique na guia `Usuários`, selecione o grupo `Usuários Sem Privilégios de Administrador` e clique em `OK`.
6. Aperte em `Concluir` e depois em `OK`. Com isso, irá retornar a janela `Console1`, vá no menu `Arquivo` clique em `Salvar como...`, mude o nome do arquivo para `gpo_admin` e deixe o `Tipo` como `Console de Gerenciamento Microsoft (*.msc)`, salve o arquivo preferencialmente em Downlodas.

Dessa forma o arquivo para editar as Políticas de Grupo estará criado.

## Configurando as Políticas de Grupo - Que afetam diretamente o usuário
A seguir são indicadas algumas gpo para o Windows 11 em computadores de uso administrativo no ICSA.
### 1 - Proibir acesso as painel de configuração 
- `Configuração do Usuário` -> `Modelos Administrativos` -> `Painel de Controle` -> `Proibir acesso ao Painel de Controle e às configurações do PC` - Selecione `Habilitado`

### 2 - Setando um papel de parede da Área de Trabalho e impedindo alteração
- `Configuração do Usuário` -> `Área de Trabalho` -> `Active Desktop` - Selecione `Habilitado`
- `Configuração do Usuário` -> `Área de Trabalho` -> `Papel de Parede da Área de Trabalho` - Selecione `Habilitado`
  - Em `Nome do papel de parede` cole o caminho `C:\Users\Public\Pictures\icsa-default.jpg`. Obs: copie a imagem `icsa-default.jpg` para `C:\Users\Public\Pictures\` com antecendência.

### 3 - Ocultar o Disco C no Explorador de Arquivos
- `Configurações do Usuário` -> `Modelos Administrativos` -> `Componentes do Windows` -> `Explorador de Arquivos` -> `Ocultar estas unidades especificadas em Meu computador` - Selecione `Habilitado`
  - Em `Selecionar uma das seguintes combinações` escolha `Restringir apenas a unidade C`
- `Configurações do Usuário` -> `Modelos Administrativos` -> `Componentes do Windows` -> `Loja` -> `Desabilitar o aplicativo Store` - Selecione `Habilitado`
- `Configurações do Usuário` -> `Modelos Administrativos` -> `Componentes do Windows` -> `Conteúdo de Nuvem` -> `Configurar Destaque do Windows na tela de bloqueio` - Selecione `Desabilitado`

## Configurando as Políticas de Grupo - Que efetam o sistema por inteiro
Para isso será necessário editar diretamente pelo `gpedit`. Então, abra o executar do windows (`WIN+R`) e digite `gpedit.msc`.
### 1 - Impedindo que o usuário adicione conta Microsoft (Outlook) no windows
- `Configurações do Comutador` -> `Configurações do Windows` -> `Configurações de Segurança` -> `Políticas locais` -> `Opções de Segurança` -> `Contas: bloquear contas da Microsoft` - Selecione `Os usuários não podem adicionar ou fazer logon com contas da Microsoft`

Após configurar todos as regras anteriores, para que Windows aplique as alterações é necessários abrir PowerShell ou Windows Terminal como administrador e executar o comando `gpupdate.exe /force`. Após as políticas serem efetuadas reinicie o sistema.
