# Implantação do Wordpress com Docker e AWS
# Descrição
O projeto tem como objetivo a criação de uma aplicação Wordpress em uma instância Amazon EC2 com ambiente baseado em Linux utilizando Docker-Compose junto de um banco de dados Wordpress em um container RDS. Além da instância principal, o Docker-Compose e o Wrodpress, também foram necessários criar uma instância a mais para funcionar como Bastion Host para o acesso da principal, uma VPC para as instâncias e um Load Balancer para a organização do tráfego. Também foi usado EFS para o armazenamento estático dos arquivos.
# Pré-requisitos
1.	Possuir uma conta na AWS com permissões suficientes para criar e configurar os serviços mencionados.
2.	Conhecimento básico de Docker e containers.
3.	Conhecimento básico de Git e repositórios.
# Criação das Instâncias e Instalação e Configuração do Docker e Docker-Compose
1.	Criação de uma VPC para o projeto.
2.	Criação de uma instância EC2 pública com ambiente AWS Linux para servir como bastion host.
3.	Criação de uma instância EC2 privada com ambiente AWS Linux 2, onde tudo foi configurado.
4.	Download do docker através do comando “sudo yum install docker” e inicialização com o comando “sudo systemctl start docker”.
5.	Download e configuração do docker-compose através dos seguintes comandos: "sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose", "sudo chmod +x /usr/local/bin/docker-compose", e "mv /usr/local/bin/docker-compose /bin/docker-compose"
# Upload no GitHub
1.	Criação de um repositório destinado para o trabalho.
2.	Inicialização do Git usando o comando “git init”.
3.	Comando “git status” para verificar se há arquivo(s) não adicionado(s) e comando “git add “[nome do arquivo]”” para adicioná-lo(s).
4.	Configuração do git com os comandos “git config --global user.email “[email]”” e “git config --global user.name “[nome]””.
5.	Commit através do comando “git commit -m “nome do commit””.
# Uso do Docker-Compose
1.	Criação de uma pasta chamada “Docker” e de um arquivo “docker-compose.yml”.
2.	Execução do script através do comando “docker-compose up -d” em segundo plano.
3.	Script:

version: "3.1"

services:
  wordpress:
    image: wordpress:latest
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: database-1.cforvibooekg.us-east-1.rds.amazonaws.com
      WORDPRESS_DB_USER: admin
      WORDPRESS_DB_PASSWORD: admin123
      WORDPRESS_DB_NAME: bancodedados
    volumes:
      - "/home/ec2-user/efs:/var/www/html"

4.	Acesso através do navegador para verificar se o Wordpress está funcionando.
# Configuração do EFS
1.	Criação seguindo os requisitos necessários do EFS.
2.	Criação na instância de uma pasta chamada “efs”.
3.	Configuração através dos seguintes comandos: "sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-04f0574ed6f45c865.efs.us-east-1.amazonaws.com:/ /home/ec2-user/efs" e "sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 10.0.159.83:/ /home/ec2-user/efs"
# Configuração do Load Balancer
1.	Acesso ao serviço EC2.
2.	Criação do Classic Load Balancer:
2.1.	Configuração do LB (nome, VPC, security groups...)
2.2.	Ativar as verificações de integridade de instâncias.
2.3.	Seleção das instâncias que o LB irá atuar e habilitar o balanceamento de carga.
2.4.	Configuração do LB para permitir a saída tráfego de internet.
# Configuração do Auto Scaling
1.	Criação do Auto Scaling Group de acordo com os requisitos necessários.
2.	Configuração do Auto Scaling para a criação de instâncias semelhantes à instância privada para caso ela caia ou seja interrompida.
