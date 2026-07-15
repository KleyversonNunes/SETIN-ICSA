# Guia de Instalação do Programa DCTF Mensal
O presente programa é de uso exclusivo para o Laboratório de Informática da Faculdade de Ciências Contábeis. Devido ser um programa legado da Receita Federal e que precisa de ajustes precisos de configuração do sistema para funcionar adequadamente, o seguinte guia deve ser seguido para evitar problemas na utilização do programa.

## Instalação
O instalador oficial do programa pode ser obtido no seguinte [link](https://servicos.receita.fazenda.gov.br/publico/programas/dctf-pgd/dctfmensalv3-9.exe). A instalação é bem simples, executando a partir do perfil administrador (`setin`) basta ir confirmando o que o instalador for solicitando.

## Ajustes no Windows
Alguns ajustes nas Políticas de Grupo do Windows devem ser realizadas para que o programa funcione adequadamente.

- `Políticas Computador Local\Usuários Sem Privilégios de administrador` -> `Configuração do Usuário` -> `Modelos Administrativos` -> `Painel de Control`
    - `Proibir acesso ao Painel de Controle e às configurações do PC` -> Marcar `DESATIVADO`
    - `Mostrar apenas itens do Painel de Controle e específicos` -> Marcar `HABILITADO`
        - Na "Lista de itens do Painel de Controle permitidos", escreva: Microsoft.RegionAndLanguage
    - `Visibilidade da Página de Configurações` -> Marcar `HABILITADO`
        - Em "Visibilidade da Página de Configurões", escreva: showonly:region;regionformatting

## Criando um atalho universal
Recorte o atalho do DCTF que foi criado na Área de Trabalho do usuário `setin` e cole no seguinte caminho `C:\Users\Public\Desktop`. Ao fazer isso o atalho será de acesso para todos os usuários do sistema.
Após isso saia do perfia administrador, logue como `aluno` realize o ajuste abaixo:
- Painel de Controle -> Região (Alterar formatos de data, hora ou número) -> Configurações adicionais:
    - Guia Números -> Redefinir
    - Unidade Monetária -> Redefinir

## Executando o programa
Após todos os ajustes tente executar o programa no perfil do `aluno`, caso peça a senha de administrador e confirme. 
