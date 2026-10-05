# LLDPCLI
O lldpcli é uma ferramenta de linha de comando, que serve de interface para utilizar protocolo LLDP. Para isso, é necessário instalar o pacote `lldpd`, que está disponível para as principais distribuições Linux.

## Instalação
No Debian e suas correlatas:
```Bash
sudo apt update -y && sudo apt install lldpd -y
```

No Fedora e distros relacionadas:
```Bash
sudo dnf update -y && sudo dnf install lldpd -y
```

No Arch Linux e distros derivadas:
```Bash
sudo pacman -S lldpd
```

## Utilização
O `lldpd` opera como um _daemon_, ou seja, é um programa que executa em segundo plano sem inter:venção direta do usuário. Ele é capaz de receber e enviar quadros **LLDP**. Segundo a [Arch Wiki](https://man.archlinux.org/man/lldpd.8.en), o Link Layer Discovery Protocol (LLDP) é uma protocolo de camada 2 (Enlace) independente de fornecedor que permite que um dispositivo de rede anuncie sua identidade e recursos na rede local.
O protocolo LLDP pode ter várias utilizações, mas para fim deste guia, sua utilização será para descoberta, a partir de um host, onde este está chegando no switch correspondente. Para isso deve ser utilizado o comando:

```Bash
sudo lldpcli show neighbors
```

o comando acima exibirá informações sobre os "vizinhos" físicos ou lógicos (como switches, roteadores, APs) conectados às interfaces de rede do sistema em execução.
Antes de executar o comando acima é preciso ativar o daemon `lldpd`, para isso faça:
```Bash
sudo systemctl start lldpd
```
ou
```Bash
sudo systemctl enable lldp
```
para uma ativação permanente no Systemd.

## Resultados
Após rodar o comando de descoberta, serão exibidas informações sobre o dispositivo vizinho (switch, roteador, AP), como o nome do dispositivo, MAC, a descrição do sistema operacional em execução, entre outras. Mas, para fins desse guia, a informação mais relevante é na seção de `Port` que contém o atributo `PortID` que indica o número da porta do dispositivo vizinho onde o host está fisicamente conectado.
