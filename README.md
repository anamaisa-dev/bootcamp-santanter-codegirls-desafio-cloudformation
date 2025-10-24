# Implementando minha Primeira Stack com AWS CloudFormation

> Documentação prática do laboratório AWS CloudFormation - Formação AWS Cloud Foundation

***

## Índice

- [Introdução](#introdução)
- [O que é AWS CloudFormation](#o-que-é-aws-cloudformation)
- [Conceitos Fundamentais](#conceitos-fundamentais)
- [Estrutura de um Template CloudFormation](#estrutura-de-um-template-cloudformation)
- [Criando Stacks no CloudFormation](#criando-stacks-no-cloudformation)
- [Implementação Prática: Stack de Firewall](#implementação-prática-stack-de-firewall)
- [Insights e Aprendizados](#insights-e-aprendizados)
- [Conclusão](#conclusão)

***

## Introdução

Este repositório documenta minha experiência prática com AWS CloudFormation, serviço fundamental para automação de infraestrutura na nuvem. O objetivo foi compreender como implementar Infrastructure as Code (IaC) utilizando templates YAML/JSON, criar stacks automatizadas e aplicar conceitos de governança e segurança.

Durante o laboratório, explorei a criação de recursos AWS de forma declarativa, incluindo configurações de firewall com Security Groups, instâncias EC2 e políticas de acesso.

***

## O que é AWS CloudFormation

O AWS CloudFormation é um serviço que permite **modelar e provisionar recursos AWS** de forma automatizada e repetível através de templates. Ele oferece:

- **Infrastructure as Code (IaC):** Definir toda infraestrutura em arquivos versionáveis
- **Automação completa:** Criar, atualizar e deletar recursos em lote
- **Consistência:** Garantir ambientes idênticos entre dev, teste e produção
- **Rollback automático:** Reverter mudanças em caso de falhas
- **Auditoria:** Rastrear todas as alterações de infraestrutura

***

## Conceitos Fundamentais

### Stacks

Conjuntos de recursos AWS gerenciados como uma unidade. Toda a infraestrutura definida em um template é provisionada como uma stack.

### Templates

Arquivos em formato YAML ou JSON que descrevem os recursos desejados e suas configurações.

### StackSets

Permitem provisionar stacks em múltiplas contas AWS e regiões simultaneamente.

### Change Sets

Visualização prévia das mudanças antes de aplicá-las na stack, reduzindo riscos.

### Nested Stacks

Reutilização de templates aninhando um dentro de outro, facilitando modularização.

***

## Estrutura de um Template CloudFormation

Um template CloudFormation possui as seguintes seções principais:

### 1. **AWSTemplateFormatVersion** (opcional)

Especifica a versão do formato do template.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
```

### 2. **Description** (opcional)

Descrição do que o template faz.

```yaml
Description: 'Template para provisionar EC2 com Security Group'
```

### 3. **Parameters** (opcional)

Valores customizáveis passados durante a criação da stack.

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium
    Description: Tipo da instância EC2
```

### 4. **Mappings** (opcional)

Mapeamento de valores condicionais baseados em regiões ou ambientes.

```yaml
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0c55b159cbfafe1f0
    sa-east-1:
      AMI: ami-0a10b27219a5094d7
```

### 5. **Resources** (obrigatório)

Define os recursos AWS que serão criados.

```yaml
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]
```

### 6. **Outputs** (opcional)

Valores retornados após a criação da stack (ex: IDs, URLs).

```yaml
Outputs:
  InstanceID:
    Description: ID da instância EC2
    Value: !Ref MyEC2Instance
```

***

## Criando Stacks no CloudFormation

1. Acessar o serviço CloudFormation
2. Clicar em "Create Stack"
3. Fazer upload do template (YAML/JSON) ou usar URL do S3
4. Configurar parâmetros
5. Revisar e criar a stack

***

## Implementação Prática: Stack de Firewall

### Objetivo

Criar um Security Group com regras de firewall permitindo acesso SSH (porta 22) e HTTP (porta 80).

[Template YAML](/templates/firewall.yaml)

### Resultado

- Security Group criado automaticamente
- Regras de ingresso configuradas
- ID exportado para reutilização em outras stacks

***

## Benefícios

- **Versionamento:** Templates podem ser versionados no Git
- **Reusabilidade:** Templates podem ser reutilizados em múltiplos ambientes
- **Documentação viva:** O código documenta a infraestrutura
- **Rollback seguro:** Reversão automática em caso de falha
- **Conformidade:** Garante padrões de segurança e governança

***

## Insights e Aprendizados

### O que aprendi

- CloudFormation elimina trabalho manual repetitivo e reduz erros humanos
- Templates YAML são mais legíveis que JSON para IaC
- Change Sets são essenciais para ambientes produtivos
- A integração com outros serviços AWS (CloudTrail, CloudWatch) fortalece governança

### Desafios encontrados

- Compreender a sintaxe de funções intrínsecas
- Gerenciar dependências entre recursos
- Validar CIDRs e permissões corretas em Security Groups

***

## Conclusão

O AWS CloudFormation é uma ferramenta essencial para qualquer profissional que trabalha com infraestrutura na AWS. Durante este laboratório, compreendi como:

- Definir infraestrutura como código usando templates YAML  
- Criar e gerenciar stacks automaticamente  
- Implementar configurações de firewall com Security Groups  
- Aplicar boas práticas de governança e versionamento  
- Utilizar parâmetros, mappings e outputs para templates reutilizáveis  
