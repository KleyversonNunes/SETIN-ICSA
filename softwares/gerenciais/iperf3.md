# Utilizando o IPERF3 para medir desempenho de redes
O iperf3 é uma ferramenta de linha de comando que pode ser utilizada para medir o **_throughput_** (velocidade real) de uma conexão de rede. Está disponível para Linux, Windows e Mac, esse breve tutorial se baseia na execução do programa no Linux, mas pode ser replicado para os demais sistemas.

O iperf3 está disponível para instalação nas principais distribuições Linux. A seguir veja como realizar a instalação:

Nas distribuições base Debian/Ubuntu:
```Bash
sudo apt update -y && sudo apt install iperf3 -y
```
Para as distribuições base Fedora/RedHat:
```Bash
sudo dnf update -y && sudo dnf install iperf3 -y
```
Para as distribuições base Arch:
```Bash
sudo pacman -S iperf3
```

## Executando o servidor
O iperf3 contém funcionalidades tanto de cliente como servidor. Para o funcionamento básico, um computador deve ser estabelecido como servidor. Para isso, utiliza-se o seguinte comando:
    
```Bash
iperf3 -s
```

Por padrão, o iperf3 utiliza a porta `5201`, mas é possível especificar uma porta utilizando o parâmetro `-p`:
```Bash
iperf3 -s -p <porta>
```

## Executando o cliente
Ao executar em um computador em modo de servidor, este ficará escutando a porta padrão ou a especificada por clientes. Para utilizar o iperf3 em modo cliente deve-se utilizar o parâmetro `-c` e na sequência especificar o endereço `ip` do servidor:
```Bash
iperf3 -c <ip_server>
```

No caso do servidor está sendo executado em uma porta diferente da padrão, também é possível especificar a porta utilizando o mesmo parâmetro `-p`:
```Bash
iperf3 -c <ip_server> -p <porta>
```

## Dicas Adicionais (Parâmetros úteis)
Teste no modo reverso (-R): Por padrão, o iperf3 testa o tráfego do cliente para o servidor (upload). Para testar a velocidade do servidor para o cliente (download), adicione -R:

```Bash
iperf3 -c <ip_server> -R
```

Teste de tráfego UDP (-u): Útil para validar redes que utilizam tráfego em tempo real, como sistemas VoIP. Por padrão, ele testa TCP. Para UDP, use:

```Bash
iperf3 -c <ip_server> -u
```
## Problema comum
Um problema comum ao tentar iniciar a comunicação com o servidor é o teste simplesmente não conectar. A causa mais frequente desse comportamento é o bloqueio da porta no firewall.

Por exemplo, para desbloquear a porta padrão do iperf3 no UFW, deve ser executado o seguinte comando:

```Bash
sudo ufw allow 5201/tcp
```
_(Caso o servidor esteja sendo executado em uma porta diferente da padrão, ajuste o número da porta no comando anterior.)_

Após liberar a porta é importante recarregar o firewall, para isso faça:
```Bash
sudo ufw reload
```
Para confirmar se a porta foi liberada, verifique a lista de liberação da seguinte forma:
```Bash
sudo ufw status
```
