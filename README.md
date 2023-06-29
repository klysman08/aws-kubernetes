# aws-kubernetes
# AWS

<aside>
💡 Console sign-in: https://244492811958.signin.aws.amazon.com/console

User name
password 

</aside>

# VPC (Virtual Private Cloud)

1. Crie uma nova VPC no painel
2. Crie uma subnet privada e uma publica
    1. #privada
    zona: us-east-1a
    bloco CIDR ipv4
    10.0.0.0/24
    2. #publica
    zona: us-east-1a
    bloco CIDR ipv4
    10.0.1.0/24

## Criando maquinas EC2 nas sub-nets

1. Crie uma nova instancia EC2
2. Escolha a imagem ubuntu
3. tipo t2.micro
4. crie um par de chaves shh .pem
5. escolha a vpc criada
6. escolha a subnet privada ou publica em funçao do EC2 criado
7. atribua um ip publico ou não em funçao da subnet/ec2

## Crie um I**nternet gateways**

para acessar via ssh as maquinas

1. Vincule o intenet gatway a uma VPC (clique em associar menu acoes)
2. Crie uma route table publica 
3. Edit routes → 0.0.0.0/0 → internet gateway
4. Associe uma subnet na tabela de rotas
5. Crie uma route table privada

## Acessando a maquina

1. Conecte  com a chave ssh
    1. ssh -i .ssh/imersao.pem [ubuntu@](mailto:ubuntu@18.232.81.134)IP
    2. caso dê problema: `chmod 400 ssh -i .ssh/imersao.pem`
    3. cole a chave ssh local na maquina publica:
        1. `scp -i ~/.ssh/imersao.pem /home/klysman/.ssh/imersao.pem [ubuntu@18.232.81.134](mailto:ubuntu@18.232.81.134):/home/ubuntu/.ssh/imersao.pem
        imersao.pem`
    4. use o endereço privado da maquina privada para acessar da maquina pública 
    5. `ssh -i ~/.ssh/imersao.pem [ubuntu@10.0.0.75](mailto:ubuntu@10.0.0.75)`

## NAT gateway

para uma maquina somente acessar conteudos externos

1. Crie uma nova NAT gateway
2. Esolha a sub-net publica
3. Tipo ne conexao publica
4. Aloque um Elastic IP
5. Atualize a tabela de rotas privada
    1. Edit as rotas:
        1. 0.0.0.0/0 → NAT gateway

# CloudFormation

AWS CloudFormation provides a common language to describe and provision all the infrastructure resources in your environment in a safe, repeatable way.

## Crie novas funções (roles)

1. EKS
2. CNI
3. WorkerNode
4. Ec2Container readonly

## Criando uma stack

1. Templete is read
2. Amazon s3 URL
    1. [`https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml`](https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml)

# **Elastic Kubernetes Service (Amazon EKS)**

1. Crie um novo cluster
2. Selecione a rede eks-imersao-net-VPC
3. Selecione o grupo de seguranca
4. Avance até o final com as configurações padrões
    1. esperar 15 min
5. Adicione um grupo de nós:
    1. Amazon Linux 2
    2. On-demand
    3. T3.medium
    4. 2-2-3
    5. Somente redes privadas.

# AWS CLI

[Installing or updating the latest version of the AWS CLI - AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#cliv2-linux-install)

```bash
aws configure

autenticação key
```

```bash
aws eks update-kubeconfig --name eks-imersao
```

```bash
kubectl apply -f k8s/
```
