# Atividade-AWS-_Docker--Compass_UOL-Public

![316685321-88a5abf3-9e83-4267-9cab-579f9aab3826](https://github.com/user-attachments/assets/dd0ac0a8-7462-43b7-bcbd-6fc77f111976) 
# ESCOPO: 📜
Instalação e configuração do DOCKER ou CONTAINERD no host EC2; Ponto adicional para o trabalho utilizar a instalação via script de Start Instance (user_data.sh)

Efetuar Deploy de uma aplicação Wordpress com container de aplicação, RDS database Mysql

configuração da utilização do serviço EFS AWS para estáticos do container de aplicação Wordpress

configuração do serviço de Load Balancer AWS para a aplicação Wordpress

# Observar com atenção 🚨:
> Não utilizar ip público para saída do serviços WP (Evitem publicar o serviço WP via IP Público)

> Sugestão para o tráfego de internet sair pelo LB (Load Balancer Classic)

> Pastas públicas e estáticos do wordpress sugestão de utilizar o EFS (Elastic File Sistem)

> Fica a critério de cada integrante usar Dockerfile ou Dockercompose;

> Necessário demonstrar a aplicação wordpress funcionando (tela de login)

> Aplicação Wordpress precisa estar rodando na porta 80 ou 8080;

> Utilizar repositório git para versionamento;

# 🚩1. > CRIAR A VPC:
Na aba de pesquisa 🔍 "Search" da Aws Pesquise por "VPC":
![VPC](https://github.com/user-attachments/assets/261e5d04-a3be-42ba-a934-477aa6fdeaa1)

> Em seguida configure duas SubNets (Pública e Privada) ambas para duas zonas de disponibilidades (us-east-1a e us-east-1b)

![VPC01](https://github.com/user-attachments/assets/689e9731-c1bc-4398-9d37-715cac9c9608)

![VPC2](https://github.com/user-attachments/assets/38e7e23a-25aa-45b7-81aa-76fddf29dbb3)

> Ao finalizar clique em "Mapa de resursos da VPC" o resultado será conforme abaixo:
![VPC03](https://github.com/user-attachments/assets/c0b47f3d-5e10-477d-bcd3-ad62c5822f91)

 # Criando Internet Gateway
 Vá ate o menu de Internet Gateway e clique em Create Internet Gateway
 De um nome de sua preferência, e associe-o á nossa VPC criada anteriorimente.

# Criando NAT gateway
Ainda no menu de VPC, clique NAT Gateway e depois em Create Nat gateway
De um nome de sua escolha, selecione uma sub-net publica, criada anteriormente e em Connectivity type deixe como público.
Por fim associe um elastic IP e crie a NAT.

# Criando Sub-nets
No menu de VPC ainda, vá em subnets e depois em Create subnet
Crie duas sub-redes, uma pública e uma privada, elas precisam estar na mesma zona de disponibilidade. Repita o processo para a segunda zona de diponibilidade;

# Criando tabela de rotas
Crie uma Route table em route tables e depois em create route table
Crie duas tabelas, uma para sub-nets privadas e outra para sub-nets públicas.
Depois associe cada sub-net a sua respectiva tabela, privada na tabela privada e publica na tabela publica.
Selecione a tabela privada e clique em Edit Routes o Destinatinatio deve ser 0.0.0.0/0 e o Target deve ser o NAT Gateways, o mesmo que criamos a pouco.
Selecione a tabela publica e clique em Edit Routes o Destinatinatio deve ser 0.0.0.0/0 e o Target deve ser o internet gateways, o mesmo que criamos a pouco.

# 🚩2. > CRIAR OS SECURITY GROUPS:
> No menu EC2 procure por 🔍  (Security groups) na barra de navegação à esquerda.
> Acesse e clique em (Criar novo grupo de segurança), e crie os grupos de segurança a seguir.

![LOAD](https://github.com/user-attachments/assets/a6b48789-5093-4b41-96ac-c0b12be58d53) 

# 🚩3. > CRIAR O RDS:
> Busque por RDS na Amazon AWS.
> Na página de RDS clique em Create database:

![RDS](https://github.com/user-attachments/assets/7fc1b12a-900d-4abc-a1b8-bce86622aa4c)
![RDS02](https://github.com/user-attachments/assets/370be953-aa8d-47e5-998b-c30d3f285a3c) 

> Em Engine options selecione MySQL

![01](https://github.com/user-attachments/assets/6bfd03cf-4b47-4a64-ad15-d0accf79e42f)

> Em Templates selecione Free tier

![02](https://github.com/user-attachments/assets/acc46b88-f271-4bbf-b6f1-fdcb0616fa38)  

> Em Settings dê um nome identicador para o DB.

> Escolha um username.

> Escolha uma senha.

 ![03](https://github.com/user-attachments/assets/ade8e6db-7330-46ff-b9c5-c89c4314b9aa) 

> Selecione a VPC criada.
> Selecione o SG-RDS.

![04](https://github.com/user-attachments/assets/42746e0a-31cb-4d65-aded-d3072657b59d)

> Em Additional configuration dê um nome inicial ao DB
> E Finalize a criação do RDS em Create database

![05](https://github.com/user-attachments/assets/686463b0-1fa9-4439-8c74-e35261e751c8) 

![06](https://github.com/user-attachments/assets/1c85c040-e85c-493a-b417-7c3cb657fe17) 

# 🚩4. > CRIAR O EFS: 

Na aba de pesquisa 🔍 "Search" da Aws Pesquise por "EFS":

E em seguida clique em (Create file system)

![07](https://github.com/user-attachments/assets/4d73870e-4b99-4b8e-b2c8-4fc0f60b0267) 

> E depois em Costomize

![08](https://github.com/user-attachments/assets/5559a047-2601-4c06-a3f9-269ca70404be)

> Selecione o Security Group do EFS e finalize.

![09](https://github.com/user-attachments/assets/fa80744d-adfb-4178-9ed1-006400fa9d14)

# 🚩5. > CRIAR A INSTANCIA EC2: 

(CRIAR A INSTANCIA EC2) 

![10](https://github.com/user-attachments/assets/4eeac81f-c0aa-4aa9-8946-62db0f394805) 

> Vá até o painel EC2 da Amazon

> Selecione Launch Instance

> Selecione a imagem Amazon Linux 2

> Selecione o tipo t3.small

> Selecione a VPC que criamos anteriormente

> Crie uma nova chave .pem

> Clique em edit network, selecione a VPC anteriormente já criada;

> Selecione a Subnet pública 1a e habilite o endereçamento de ip público;

> Após, selecione o Security Group da Instancia Ec2;

# 🚩6. > CRIAR A AMI A PARTIR DA EC2: 

> Vá até o serviço de EC2 no console AWS e acesse as instancias.

> Selecione a instância previamente criada, clique com o botão direito sobre e vá em > "Image and Templates" > "Create Image"; Nomeie e finalize a criação.

# 🚩7. > CRIAR O TEMPLATE DA EC2: 

![11](https://github.com/user-attachments/assets/01222645-6a9c-4a87-b117-14369b5c4bb9) 

> No user data que fica em Advanced Details iremos adicionar o seguinte script:

#!/bin/bash
# Os comandos abaixo consiste em:
# Atualizar o sistema 
sudo yum update -y
# Instalar o docker
sudo yum install docker -y
# Iniciar o docker
sudo systemctl start docker
# habilitar o docker ao iniciar a instância
sudo systemctl enable docker
# Habilitar o usuário atual ao grupo do docker
sudo usermod -aG docker ec2-user
# curl no docker-compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /bin/docker-compose
#Permissões do diretório docker-compose
sudo chmod +x /bin/docker-compose
# Criar diretório do nfs com as permissões de acesso
sudo mkdir -m 777 /home/ec2-user/efs
#Instalar o nfs
sudo yum install amazon-efs-utils -y
# Montar o Nfs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-02ea641eed87bfa65.efs.us-east-1.amazonaws.com:/ /home/ec2-user/efs
# habilitar a montagem automática do Nfs 
echo "fs-02ea641eed87bfa65.efs.us-east-1.amazonaws.com:/ /home/ec2-user/efs nfs defaults 0 0" >> /etc/fstab
# Montar todos os arquivos que estiverem no /etc/fstab
sudo mount -a
                                                                                                                         
#Docker-compose
cat <<EOL > /home/ec2-user/efs/docker-compose.yaml
version: '3.8'
services:
  wordpress:
    image: wordpress:latest
    volumes:
      - /home/ec2-user/efs/wordpress:/var/www/html
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_HOST: seu ENPOINT
      WORDPRESS_DB_USER: seu usuário
      WORDPRESS_DB_PASSWORD: sua senha
      WORDPRESS_DB_NAME: nome do seu blog
EOL

docker-compose -f /home/ec2-user/efs/docker-compose.yaml up -d 


# 🚩8. > CRIAR O TARGET GROUP:
> No menu de Load Balancing, abaixo dele clique em Target Groups

> Depois em Create target Group

> Selecione Instances

> Selecione um nome

> Selecione a VPC criada anteriormente e o resto deixaremos como está

> Clique em next e create

# 🚩9. > CRIAR O CLASSIC LOAD BALANCER: 

![12](https://github.com/user-attachments/assets/7442fdc6-15a3-416d-8e10-d11a9354f14d)

> Clique no menu a esquerda Load Balancing e depois em Create Load Balancer

> Depois em Classic Load Balancer

> De um nome que desejar.

> Na opção scheme deixe em Internet-facing

> Em IP address type deixe em IPv4

> Associe a VPC criada anteriormente.

> Selecione duas AZs

> Selecione o SG criado anteriormente e por fim confirme a criação do LB.


# 🚩10. > CONFIGURAR O AUTO SCALING:

> No menu EC2 procure por Auto Scaling na barra de navegação à esquerda.

> Acesse e clique em Creat Auto Scaling group.

> Nomeio o grupo de Auto Scaling.

> Selecione o modelo de execução criado anteriormente.

> A seguir clique em Next.

> Selecione a VPC criada anteriormente.

> Selecione as Sub-redes Privadas.


![13](https://github.com/user-attachments/assets/fd3d7cbd-7ab2-408f-9d43-d35c3be2af1d) 

> Clique em Next.

> Marque a opção Attach to an existing load balancer.

> Marque a opção Choose from your load balancer target groups.

> Selecione o grupo de destino criado anteriormente.

> A seguir clique em Next.

> Em Group size selecione:
  ( Capacidade desejada: 2 )
  (  Capacidade mínima: 2 )
  ( Capacidade máxima: 3 )

> A seguir clique em Skip to review. 

> Clique em Creat Auto Scaling group.

# TESTE DA PÁGINA INICIAL DO WORDPRESS PELO DNS DO LOAD BALANCER: 


> Copie o nome do DNS e cole no navegador :

![14](https://github.com/user-attachments/assets/5dc60d1c-c100-4b40-a5ac-6adaf678c3d3) 


![15](https://github.com/user-attachments/assets/653bfec9-4bc3-4c9e-9ae4-83a25d8d74cf)




