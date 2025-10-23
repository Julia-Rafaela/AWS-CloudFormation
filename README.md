O que é o CloudFormation?
Em vez de ir no site da AWS e ficar clicando pra criar uma máquina, um bucket, ou uma VPC, você escreve tudo isso num arquivo de texto (usando YAML ou JSON). Aí você entrega esse arquivo pro CloudFormation, e ele cuida de criar tudo automaticamente pra você.
Isso é o que chamamos de Infraestrutura como Código (IaC).

Por que usar isso?
Automatiza a criação dos recursos
Você evita erros de configuração manual
Pode repetir a mesma infra várias vezes (ótimo pra dev, teste e produção)
Pode versionar junto com o código da aplicação
Facilita a manutenção da infra (tá tudo documentado no código)

Como o CloudFormation funciona?
Você cria um arquivo chamado “template”. Esse template descreve os recursos que você quer (como EC2, S3, etc). Quando você manda esse template pra AWS, ele vira uma stack, que é como se fosse um “pacote” com tudo o que vai ser criado.

Exemplo bem simples de template:
AWSTemplateFormatVersion: '2010-09-09'
Description: Criando uma instância EC2 simples

Resources:
  MinhaInstancia:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0abcdef1234567890


Esse código diz: “AWS, me dá uma instância EC2 do tipo t2.micro com essa imagem”.

As partes mais importantes que você precisa conhecer:
1. Resources
Aqui você coloca os recursos que quer criar. Essa parte é obrigatória.

2. Parameters
Se você quiser deixar o template mais flexível, pode pedir valores como entrada. Por exemplo: tipo da instância, nome do bucket, etc.

Parameters:
  TipoDeInstancia:
    Type: String
    Default: t2.micro

3. Outputs
Depois que tudo for criado, você pode mostrar alguns valores importantes, como o IP público de uma instância.

Outputs:
  IPPublico:
    Value: !GetAtt MinhaInstancia.PublicIp

4. Mappings
Serve pra mapear valores fixos, tipo: se tiver em tal região, usa essa AMI.

5. Conditions
Permite criar recursos só se uma condição for verdadeira. Exemplo: só cria o bucket se o parâmetro “CriarBucket” for “sim”.

Como usar isso na prática?

Você escreve o template
Valida o template (pra ver se tem erro)
Usa o console da AWS ou a linha de comando (CLI) pra criar a stack
O CloudFormation cria tudo automaticamente
Quando quiser mudar, você edita o template e roda um update
Quando não precisar mais, deleta a stack e ele remove tudo

Comandos básicos (pra treinar com a AWS CLI)
# Validar o template
aws cloudformation validate-template --template-body file://template.yaml

# Criar uma nova stack
aws cloudformation create-stack --stack-name minha-stack --template-body file://template.yaml

# Atualizar a stack
aws cloudformation update-stack --stack-name minha-stack --template-body file://template.yaml

# Deletar a stack
aws cloudformation delete-stack --stack-name minha-stack

Considerações sobre Segurança e Boas Práticas
Princípio do Menor Privilégio: Defina permissões mínimas para acessar recursos.
Utilização de Parâmetros Sensíveis: Para senhas ou informações confidenciais, utilize o AWS Secrets Manager ou SSM Parameter Store para gerenciar dados sensíveis.
Automação de Testes: Teste seus templates regularmente para garantir que eles criam a infraestrutura de forma segura e sem erros.
