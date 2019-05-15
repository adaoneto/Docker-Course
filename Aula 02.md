# Aula 02



#### Dockerfile

- **FROM** Distribuição; Versão
- **COPY** [arquivo local] ←→ [diretório/arquivo no container]
- **RUN** comando
- **EXPOSE** porta do seriço
- **cmd** [comando executado ao iniciar o container]

#### DockerHub

#### Cópia de Arquivos

1. Para copiar um arquivo do container em execução para a máquina local, execute os seguinte comandos:

   > docker cp [nome ou id do container]:[arquivo do container] [diretório local]

2. Para copiar um arquivo da máquina local para um container em execução, execute os seguintes comandos:

   > docker cp [arquivo local] [nome ou id do container]:[diretório no container]

---

##### Laboratório 01

2. Verifique se o container servidor-debian está em execução. Caso não esteja, use o comando docker run:

   > docker ps

3. Para copiar um arquivo do container em execução para a máquina local, execute os seguintes comandos:

   > docker exec servidor-debian cat etc/hosts
   >
   > docker cp servidor-debian:/etc/hosts /tmp
   >
   > cat /tmp/hosts

4. Para copiar um arquivo da máquina local para um container em execução, execute os seguintes comandos:

   > docker cp /etc/hosts servidor-debian:/tmp
   >
   > docker exec servidor-debian cat tmp/hosts

---

#### Backup e Restore

1. Faça o backup do container em execução servidor-debian:

   > docker export [nome ou id do container] > [backup].tar

2. Antes de restaurar o backup, remova o container.

3. Importe o container através do seguinte comando:

   > cat [backup].tar | docker import - [backup]

---

##### Laboratório 02

1. Faça o backup do container em execução servidor-debian:

   > docker export servidor-debian > servidor-debian.tar
   >
   > docker du -sh servidor-debian.tar

2. Antes de restaurar o backup, remova o container servidor-debian através do seguinte comando:

   > docker rm -f servidor-debian

3. Importe o container servidor-debian através do seguinte comando:

   > cat servidor-debian.tar | docker import - servidor-debian

4. Inicie o container servidor-debian:

   > docker run -d --privileged --name servidor-debian --hostname servidor-debian servidor-debian bash
   >
   > docker run -it servidor-debian bash

5. Abandone o container e o remova através dos seguintes comandos:

   > exit
   >
   > docker rm -f servidor-debian
   >
   > docker ps

