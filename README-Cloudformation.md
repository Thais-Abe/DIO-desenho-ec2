📘 README – AWS CloudFormation

🚀 O que é o AWS CloudFormation?

É uma automação que utiliza templates nas linguagens json/yaml usados para criações de recursos na AWS.É possível versionar esses templates também. 

✨ Benefícios

Infra como código → facilita versionar e compartilhar.

Automação → cria ambientes inteiros com um único comando.

Consistência → evita erros de configuração manual.

Gerenciamento simplificado → cria, atualiza e deleta pilhas inteiras de recursos.

📌 Estrutura básica

Um template CloudFormation é dividido em seções:

AWSTemplateFormatVersion: '2010-09-09'
Description: Exemplo simples de CloudFormation
Resources:
  MinhaBucketS3:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: meu-bucket-exemplo-thais


Neste exemplo:

Criamos um bucket S3.

Resources → define o recurso.

Type → o tipo de recurso AWS.

Properties → parâmetros específicos do recurso.

⚙️ Exemplo com EC2 e Security Group
AWSTemplateFormatVersion: '2010-09-09'
Description: EC2 com Security Group via CloudFormation
Resources:
  MeuSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Acesso SSH
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0

  MinhaEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0 # (exemplo, varia por região)
      SecurityGroups:
        - !Ref MeuSecurityGroup


Esse template cria:

Um Security Group liberando SSH (porta 22).

Uma instância EC2 t2.micro usando esse grupo.

🔧 Como usar

Salve o template como template.yaml.

No console AWS, vá em CloudFormation → Create Stack.

Faça o upload do template e siga os passos.

A AWS vai provisionar todos os recursos descritos.