# ☁️ AWS CloudFormation — Infraestrutura Automatizada como Código

## 📘 Visão Geral
O **AWS CloudFormation** é um serviço que permite **provisionar e gerenciar recursos de infraestrutura AWS automaticamente**, utilizando **modelos declarativos em YAML ou JSON**.  
Com ele, é possível criar, atualizar e versionar toda a infraestrutura como **Infraestrutura como Código (IaC)**, garantindo **padronização, automação e reprodutibilidade**.

---

🧩 Benefícios do CloudFormation

Automação total: elimina a criação manual de recursos.

Controle de versão: infraestrutura versionada junto ao código.

Reprodutibilidade: permite recriar o mesmo ambiente em diferentes contas/regiões.

Integração com CI/CD: pode ser integrado ao CodePipeline, Jenkins, GitHub Actions, etc.

🧠 Boas Práticas

Separar templates por tipo de recurso (rede, segurança, aplicação).

Utilizar exports/imports para compartilhar recursos entre stacks.

Definir Outputs úteis (como IDs de VPC, Subnets, etc).

Adicionar descrições claras em cada parâmetro e recurso.

Utilizar IAM Roles específicas com permissões mínimas necessárias.

Neste aquivo YAML abaixo esta o código para criar: 

🌐 1 VPC com uma sub-rede pública

🔒 1 Security Group permitindo acesso SSH/HTTP

💻 1 EC2 Instance rodando dentro dessa subnet

🗄️ 1 Bucket S3 criado automaticamente

🌍 1 Internet Gateway e rotas configuradas para internet

YAML:

AWSTemplateFormatVersion: '2010-09-09'
Description: Infraestrutura automatizada com VPC, Subnet, EC2 e S3.

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
        - Key: Name
          Value: DemoVPC

  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true
      AvailabilityZone: !Select [0, !GetAZs '']

  MyInternetGateway:
    Type: AWS::EC2::InternetGateway

  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref MyVPC
      InternetGatewayId: !Ref MyInternetGateway

  MyRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref MyVPC

  MyRoute:
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref MyRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref MyInternetGateway

  MySubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref MySubnet
      RouteTableId: !Ref MyRouteTable

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Permitir SSH e HTTP
      VpcId: !Ref MyVPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro
      ImageId: ami-0c02fb55956c7d316  # Amazon Linux 2 (exemplo)
      SecurityGroupIds:
        - !Ref MySecurityGroup
      SubnetId: !Ref MySubnet
      Tags:
        - Key: Name
          Value: DemoEC2

  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub my-demo-bucket-${AWS::AccountId}

Outputs:
  EC2PublicIP:
    Description: IP público da instância EC2
    Value: !GetAtt MyEC2Instance.PublicIp


