# Servidor-PXE-
Servidor de Formação por rede PXE com [FOG Project](https://fogproject.org/)


## A Importância do Servidor PXE em Ambientes Corporativos

Em um mundo cada vez mais digital, as empresas dependem de soluções de TI eficientes e escaláveis para garantir a produtividade e minimizar o tempo de inatividade. Uma das tecnologias que tem ganhado destaque nesse cenário é o **Servidor PXE (Preboot Execution Environment)**. Este protocolo permite que computadores se iniciem através da rede, eliminando a necessidade de mídias físicas como CDs ou pen drives para a instalação de sistemas operacionais. 

Uma das principais razões para a adoção do PXE nas empresas é sua capacidade de simplificar o processo de **implantação de sistemas operacionais**. Em ambientes corporativos, especialmente aqueles com grandes quantidades de estações de trabalho, configurar cada máquina individualmente pode ser uma tarefa exaustiva e suscetível a erros. Com o PXE, um único servidor pode gerenciar e implantar imagens de sistema operacional em múltiplas máquinas simultaneamente, economizando tempo e recursos.

Além disso, o uso de um servidor PXE contribui significativamente para a **escalabilidade** da infraestrutura de TI. À medida que uma empresa cresce, o número de dispositivos a serem gerenciados aumenta. O PXE permite que novas máquinas sejam integradas à rede de forma rápida e eficiente, sem as etapas manuais que seriam necessárias em processos tradicionais. Isso é especialmente benéfico em empresas que frequentemente adicionam ou substituem equipamentos.

Outro aspecto importante é a **recuperação e manutenção de sistemas**. No caso de uma falha no sistema operacional ou a necessidade de reinstalação, o PXE fornece uma solução prática e rápida, permitindo que as máquinas sejam recuperadas sem a necessidade de intervenção física. Essa capacitação não só diminui o tempo de inatividade, mas também ajuda a manter a continuidade dos negócios.

Em resumo, a implementação de um servidor PXE em uma empresa proporciona uma série de vantagens, como a **eficiência no gerenciamento de sistemas**, **escalabilidade**, e **facilidade de manutenção**. À medida que as empresas continuam a evoluir em direção a um futuro mais digital, a adoção de tecnologias como o PXE se torna não apenas desejável, mas essencial para o sucesso no dinâmico ambiente empresarial.


### Pré-requisitos

1. Servidor Linux: Uma das distribuições recomendadas é o Ubuntu Server, aqui usaremos o Lubuntu 25.10.
2. Acesso à Rede: Certifique-se de que o servidor tenha uma conexão de rede estável.
3. Ambiente LAMP: É necessário ter o Apache, MySQL e PHP instalados.


- atualize o sistema
```bash
sudo apt update
sudo apt upgrade
```
- instale o tftpd-hpa
```bash
sudo apt install tftpd-hpa
```

- depois configure



- instale os pacotes
```bash
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql php-curl php-xml php-gd php-json tftpd-hpa
```

- Clonar o repositório do Fog Project
```bash
git clone https://gitlab.com/FOG/FOG.git
cd FOG
./bin/installfog.sh
```

Depois siga suas instruções e configure da melhor forma
Ele instala:

- O servidor web (Apache + PHP)
- O banco de dados (MariaDB/MySQL)
- O TFTP (para PXE boot)
- O NFS ou FTP (para armazenar imagens)
- O serviço de gerenciamento do FOG (FOGService)


Lembre-se, deve se configurar o dhcp da sua rede para direcionar a inicialização das maquinas para o servidor PXE.

Caso use dnsmasq altere seu arquivo de configuração.
```bash
# /etc/dnsmasq.conf
# Defina a interface a ser usada no meu caso é eth0
interface=eth0

# Configurações do DHCP coloque a mascara de sua rede
dhcp-range=192.168.1.50,192.168.1.150,12h
dhcp-boot=pxelinux.0
pxe-service=0,"PXE Boot",pxelinux.0

# Endereço do servidor TFTP
dhcp-option=66,192.168.1.10  # => Substitua pelo IP do seu servidor PXE
dhcp-option=67,pxelinux.0
```

- depois rode:
```bash
sudo systemctl restart dnsmasq
```


Se na sua rede tem algum groupware, apenas aponte o tftp para o ip<addr> do servidor PXE.


```
```
