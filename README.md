<!-- Author: Ricardo Costa A. Santos
Description: Repositório criado para armazenar informações sobre os estudos de AWS
Ano: 2021
Mês: Abril
Versão: 1.0.0

-->

# Cabeça na nuvem

#Objetivo:

Repositório criado para raa armazenar anotações do curso [cabeça na nuvem](https://cabecananuvem.com/proposito)

### Pontos de atenção:

- [x] verificar a Região desejada: Recomendado utilizar **norte virgínia** pois esta região é onde qualquer novo serviço que é lançado pela Amazon começa por ela.

# EC2

## EC2 - Compute

**IaaS - Infraestrutura as a Service**
**Unmanaged Service** Gerenciamento de recurso, disponibilidade, segurança, network, monitoramento, etc é responsabilidade do administrador e não AWS.

- Antes de criar uma EC2, precisamos criar uma _key-pair_(chave de pares)

  - Clique em _create key pair_
  - Atribua um nome para ela, selecione o formato da chave que será baixado (eu selecionei **pem**)
  - Um arquivo será baixado na sua máquina

- Key pair é uma chave que tem como **objetivo** manter as nossas instâncias seguras, sem ela nínguem poderá acessar ou logar uma vez que eles não possuem as novas chaves.

### **Acessando as instâncias**

Para acessar as instancias, clique em **running instances** no menu _EC2 Dashboard_
Neste painel, podemos ver todas as instancias já criadas e quais os status (em execução, paradas)

### Criando a sua instância EC2

Para realizar a criação de uma nova instância, clique em **Launch Instance**.
Nesta tela podemos ver a grande _diversidade_ que podemos criar utilizando a **AWS**, como:

- [x] Windows Server
- [x] Linux
      Para este primeiro exemplo, vamos criar uma máquina **Amazon Linux 2 AMI (HVM), SSD Volume Type**

- [x] Clique em "Select"
- [x] Defina qual é o tipo da máquina que você irá lançar, é possível verificar a grande variedade de configurações de máquinas que a **AWS** disponibiliza, você pode escolher diversas configurações como máquinas com uma ou mais CPU's e 1 ou 100GB de memória por exemplo.
  - [x] Irei utilizar a máquina _t2 micro_ pois ela é **elegível ao tier free**, e tenho disponível 12 meses grátis com 750 horas de utilização. Clique em **Next: Configure Instance Details**
  - [x] Nesta página, podemos realizar várias configurações de como subiremos a máquina:
    - [x] **Number of Instances:** Defina o número de instancias que você quer lançar
    - [x] **Network:** todas as contas da AWS já vem com uma _network vpc_ criada como padrão (irei utilizar ela, pois ela já vem com algumas informações básicas de segurança já configuradas.)
    - [x] **Subnet:** a _subnet_ determina qual zona de disponibilidade essa máquina será criada (isso varia dependendo da zona), no caso irei deixar a AWS selecionar qualquer uma.
    - [x] **Auto-assign Public IP:** Vou deixar _enable_ para que o IP publico seja determinado pela própria **AWS**.
    - [x] **Shutdown behavior:** define o comportamento que a máquina irá adotar se você desejar parar ela:
      - [x] _Stop:_ A máquina firá inativa/parada
      - [x] _Terminate:_ A máquina será deletada.
    - [x] **Advanced Details>User_data:** é aonde podemos definir alguns scripts para serem executados na inicialização da máquina.
    - [x] Clique em **Next: Add Storage**
- [x] Nesta página, vamos adicionar o disco de inicialização do nosso sistema, como é uma máquina, vamos utilizar a opção padrão de 8GB e ir para a próxima tela _Tags_
- [x] Nesta página, é o espaço reservado para identificação dos serviços, clique em _Add a Tag_ e ir para a próxima tela _Configure Security Group_
- [x] Nesta página posso criar ou selecionar um security group:
  - [x] Selecione _Create a **new** security group_
  - [x] Defina um nome
  - [x] Defina uma descrição
- [x] Na proxima tela podemos revisar todos os padrões que definimos, irá aparecer um _pop-up_ para selecionar a nossa _key pair_, clique no checkbox e clique em **_Launch Instance_** e será **criada a sua máquina na AWS**.

## Acessando sua máquina AWS

### Acessando sua máquina via terminal

Para acessar sua máquina via terminal do windows, primeiro é necessário guardar a **key pair** em um local seguro, ou seja, uma pasta onde somente você tem acesso.

Após mover o arquivo para a pasta segura, acesse-a pelo terminal e execute os comandos abaixo:

### Comandos para acessar a máquina via terminal:

**Comando SSH que permite acesso á maquina AWS via terminal:**

```linux
ssh <ip_publico_da_sua_maquina> -l ec2-user -i <nome_do_arquivo.pem>
```

Irá aparecer uma mensagem falando se você deseja adicionar essa key na lista, digite `yes`.

**Caso apareça a mensagem `WARNING: UNPROTECTED PRIVATE KEY FILE! `**

Esse erro é devido ao computador entender que qualquer pessoa tem acesso à pasta onde está a sua _key pair_.

Para resolver isso, rode o comando: `chmod 400 <nome_do_arquivo.pem>`. Após isso rode o comando anterior novamente e assim você conseguirá conectar na sua **máquina AWS via terminal** 🚀

### Comandos para configurar sua máquina

Após ter logado na sua máquina via terminal, execute os seguintes comandos abaixo para realizar algumas ações:

**Elevando o nível de usuário para usuário admin**

```linux
sudo su
```

**Atualizando a máquina para a versão mais recente do linux**

```linux
yum update -y
```

**Instalando o git para poder baixar repositórios**

```linux
yum install git -yy
```

**baixando o nodejs utilizando o _curl_**

```linux
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
```

**Instalando o node, após baixa-lo no passo anterior**

```linux
sudo yum install nodejs
```

**Clonando um repositório teste**

```linux
sudo git clone https://github.com/pelicri/cris.git
```

**Rodando o arquivo _server.js_ que foi baixado do repositório**

```linux
node servidor.js
```

Após rodar este comando, a aplicação ficará rodando no servidor....

## **Acessando o seu servidor via navegador (Google Chrome)**

Clicamos na nossa instancia (no console AWS) e copiamos o link público dela. Para acessarmos ela via browser podemos realizar de duas formas:

- [x] IP
- [x] DNS

**Caso você não consiga acessar**

Este erro é devido à regra _Security Group_ estar bloqueando os acessos externos via HTTP, para liberar este acesso siga os passos:

- [x] Selecione a máquina virtual
- [x] Clique na aba _Security_
- [x] Clique em _Security groups_
- [x] Clique em _Edit inbound rules_
- [x] Clique em _Add rule_ depois, selecione **HTTP** no check-box e no campo ao lado do checkbox escrito _Custom_ digite: `0.0.0.0/0` para permitir o acesso de qualquer ip externo à sua aplicação.
- [x] Clique em _save rule_

## Criando uma máquina AWS e já subindo as configurações via script

É possível criar uma máquina e já definir para ela que execute alguns scripts logo que ela iniciar.

Para isso, na criação da máquina, em: **Advanced Details > User_data**, cole o script abaixo:

```linux
#!/bin/bash -ex
# get node into yum
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
# install node (and npm) with yum
yum -y install nodejs
# install pm2 to restart node app
npm i -g pm2@2.4.3
yum update -y
yum install git -yy
git clone https://github.com/pelicri/cris.git /home/ec2-user/bin
node /home/ec2-user/bin/servidor.js

```

Com isso, após a inicialização da máquina e configuração do _Security Group_ para liberação do acesso de qualquer ip na máquina ela já estará rodando a aplicação baixada do Git e exibindo o conteúdo dela.

---

 <p align="center">Desenvolvido com 💜 por Ricardo Costa</p>
