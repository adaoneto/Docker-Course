# Aula 01

#### Instalar o Docker

1. Instalar o yum-utils, que fornece o utilitário yum-config-manager:

   > yum install yum-utils -y

2. Em seguida configurar o repositório estável através do seguinte comando:

   > yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

3. Atualize os pacotes e instale o Docker através dos seguintes comandos:

   > yum makecache fast
   >
   > yum install docker-ce

#### Validar a instalação do Docker

1. Inicie o Docker e o ative no boot do sistema:

   > systemctl start docker
   >
   > systemctl enable docker

2. Verifique se o serviço do Docker está sendo executado:

   > systemctl status docker

3. Verifique qual é a versão do Docker instalada:

   > docker version

#### Comandos Iniciais do Docker

1. Para começar vamos exibir as informações do ambiente:

   > docker info

2. Para listar containers, imagens e redes no Docker execute os comandos:

   > docker ps
   >
   > docker images
   >
   > docker network ls

3. Para pesquisar imagens no Docker Hub, execute o comando:

   > docker search [imagem]

4. Para baixar a imagem e verificar se a mesma aparece na lista de imagens:

   > docker pull [imagem]
   >
   > docker images

5. Para executar um container e verificar se o mesmo aparece na lista de containers:

   > docker run -dit --name [nome do container] --hostname [hostname do container] [imagem]
   >
   > - d: detach - rodar o container em segundo plano e mostrar o id do container
   > - it: interactive, tty - cria uma interface interativa para o container
   >
   > docker ps

6. Para se conectar ao container em execução, execute o seguinte comando:

   > docker attach [nome ou id do container]

7. Para iniciar um container já criado:

   > docker start [nome ou id do container]

8. Uma vez conectado ao container, verifique o nome da máquina e as configurações de rede:

   > hostname
   >
   > ip a

9. Para sair do container:

   > exit
   >
   > - sai e encerra o container
   >
   > Ctrl ^p ^q
   >
   > - sai do container mas mantem ele ativo em segundo plano

10.   Para remover um container:

    > docker rm [nome ou id do container]
    >
    > docker rm -f [nome ou id do container]
    >
    > - f: force - para forçar a remoção caso o container esteja ativo
    > - $(docker ps -qa): lista todos os ids para remover todos os containers

---

##### Laboratório 01

1. Pesquisar imagem Debian no Docker Hub:

   > docker search debian

2. Baixar a imagem do servidor Debian:

   > docker pull debian

3. Executar o container Debian e verificar se o mesmo aparece na lista de containers em execução:

   > docker container run -dit --name servidor-debian --hostname servidor-debian debian

4. Conctar ao container servidor-debian:

   > docker attach servidor-debian

5. Uma vez conectado ao container, verificar o nome da máquina e as configurações do rede:

   > hostname
   >
   > ip a

6. Sair do container:

   > exit

7. Remover o container:

   > docker rm servidor-debian

---

##### Laboratório 02

1. Baixar a imgaem de servidor ssh e verificar se o mesmo aparece na lista de imagens:

   > docker image pull kaixhin/ssh
   >
   > docker images

2. Executar o container configurando sua inicialização no boot na máquina hospedeira:

   > docker run -d --name servidor-ssh --hostname servidor-ssh --restart=always kaixhin/ssh

3. A partir de agora, podemos nos conectar e desconectar do container através dos seguintes comandos:

   > docker container exec -ti servidor-ssh bash
   >
   > exit

4. Pare a execução do container:

   > docker stop servidor-ssh

---

#### Comandos Essenciais Docker

1. Para iniciar a execução de um container:

   > docker start [nome ou id do container]

2. Para parar a execução de um container:

   > docker stop [nome ou id do container]

3. Para renomear a execução de um container:

   > docker rename [nome do container] [novo nome do container]

4. Para reiniciar a execução de um container:

   > docker restart [nome ou id do container]

---

##### Laboratório 03

1. Baixe a imagem do servidor ssh e verifique se o mesmo aparece na lista de imagens:

   > docker image pull solita/ubuntu-systemd-ssh
   >
   > docker images

2. Execute o container configurando e ativando o uso de privilegios:

   > docker run --privileged -d --name servide-middleware --hostname servidor-middleware solita/ubuntu-systemd-ssh

3. A partir de agora podemos nos conectar ao container e gerenciar serviços no formato **SystemD** através da ferramenta systemctl:

   > docker exec -ti servidor-middleware bash
   >
   > systemctl restart ssh
   >
   > systemctl status ssh
   >
   > exit

4. Limpe todos os containers:

   > docker rm -f $(docker ps -qa)

---

#### Instalação do Docker em ambiente Ubuntu

<!--Máquinas 02 e 03-->

1. Antes de iniciar a instalação do Docker, é necessário ativar o suporte a repositórios https instalando os seguintes pacotes:

   > apt update
   >
   > apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common -y

2. Em seguida, baixe a chave pública para configurar o repositório do Docker e atualize a lista de pacotes:

   > curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
   >
   > add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

3. Atualize a lista de pacotes e instale o pacote docker-ce:

   > apt update
   >
   > apt install docker-ce

---

##### Laboratório 04

<!--Máquina Manager-->

1. Baixe a imagem oficial do servidor **mariadb** e **wordpress** e verifique na lista de imagens em sua máquina:

   > docker pull mariadb
   >
   > docker pull wordpress
   >
   > docker images

2. Em seguida crie o diretório 'wordpress' e acessa essa pasta:

   > mkdir wordpress
   >
   > cd wordpress

3. Execute um container **mariadb** e verifique se o mesmo aparece na lista de containers em execução:

   > docker run -e MYSQL_ROOT_PASSWORD=4linux -e MYSQL_DATABASE=wordpress --name wordpressdb -v "$PWD/database":/var/lib/mysql -d mariadb:latest
   >
   > docker ps

4. Execute um container wordpress e verifique se o mesmo aparece na lista de containers em execução:

   > docker run -e WORDPRESS_DB_PASSWORD=4linux --name wordpress --link wordpressdb:mysql -p 80:80 -v "$PWD/html":/var/www/html -d wordpress

---

#### Atualizando imagens no Docker

2. Atualize as imagens Docker através da opção commit:

   > docker commit [nome ou id do container] [nome da imagem]:[tag da imagem (opcional)]

