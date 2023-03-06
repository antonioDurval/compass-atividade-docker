# compass-atividade-docker

### Criando Tabelas de Rotas
- Vá até o serviço de [VPC](https://console.aws.amazon.com/vpc/)
- Clique em Tabelas de Rotas, criar tabela de rotas.
- <strong>Nome(opcional)</strong>: Tabela-Exemplo
- <strong>VPC:</strong> Selecione a VPC desejada.
- Clique em Criar tabela de rotas.
- Agora devemos associar as nossas sub-redes a Tabela de rotas criada(Tabela-Exemplo), mas antes devemos criar uma Internet Gateway

### Criando Internet Gateway
- Clique em Gateways da Internet
- <strong>Tag de nome: </strong>igw-exemplo
- Clique em criar gateway da internet
- Após criado devemos dar um Attach para nossa VPC, Clique em <i>Attach to VPC</i> e Selecione a VPC.
- Com isso configurado agora podemos tornar as nossas Sub-Redes criadas em Sub-Redes públicas
- Vá Até Tabelas de Rotas, selecione a Tabela desejada então edite suas Rotas -> <strong>Destino:</strong> 0.0.0.0/0 | <strong>Alvo:</strong> Gateway Criado(<strong>igw-exemplo</strong>)
- Continuamos na Tabela de Rotas e agora iremos associar as Sub-Redes criadas a essa Tabela de Rotas(<strong>Tabela-Exemplo</strong>)
- Clique em Editar associação de sub-rede e selecione as sub-redes desejadas.

## Criando o Grupo de Segurança para o Balanceador de Carga
- Selecione o serviço de EC2.
- Clique em Security Groups, criar grupo de segurança.
###  Criar Grupo de Segurança - Informações
- <strong>Nome do grupo de segurança: </strong>SG-Exemplo
- Descrição: Descrição desejada
- <strong>VPC: </strong>Selecione a VPC desejada(<strong>Minha-VPC</strong>)
- Crie as seguintes regras de Entrada:
<table>
    <tr>
      <th>Versão do IP</th>
      <th>Tipo</th>
      <th>Protocolo</th>
      <th>Intervalo de portas</th>
      <th>Origem</th> 
    </tr>
    <tr>
      <td>IPv4</td>
      <td>HTTP</td>
      <td>TCP</td>
      <td>80</td>
      <td>0.0.0.0/0</td>
    </tr>
</table>

## Alterando Grupo de Segurança
Altere o grupo de segurança Default
- Selecione o serviço de EC2.
- Clique em Grupos de Segurança.
- Selecione o Grupo de Segurança Deafult.
- Clique em Regras de Entrada, logo após vá até Editar regras de entrada
- Crie as seguintes regras de Entrada:
<table>
    <tr>
      <th>Versão do IP</th>
      <th>Tipo</th>
      <th>Protocolo</th>
      <th>Intervalo de portas</th>
      <th>Origem</th> 
    </tr>
    <tr>
      <td>IPv4</td>
      <td>SSH</td>
      <td>TCP</td>
      <td>22</td>
      <td>Meu IP</td>
    </tr>
    <tr>
      <td>IPv4</td>
      <td>NFS</td>
      <td>TCP</td>
      <td>2049</td>
      <td>sg-0bcf1504bf00e0a50</td>
    </tr>
    <tr>
      <td>IPv4</td>
      <td>HTTP</td>
      <td>TCP</td>
      <td>80</td>
      <td>sg-01161baacaf672f53</td>
    </tr>
</table>

## Criando um Grupo de Destino
- Selecione o serviço de EC2;
- Clique em Grupos de Destino;
- Clique em Create target group;
- Escolha o tipo de destino, selecione: <strong>Instâncias</strong>;
- Preencha o Target group name;
- Selecione a VPC desejada;
- Vá até Configurações avançadas de verificação de integridade e no campo Códigos de sucesso informe: <strong>200,302</strong>
- Clique em proximo;
- Clique em criar grupo de destino;
- Após ter criado o Grupo de destino, criaremos o nosso Balanceador de carga.

## Criando um Balanceador de Carga
- Selecione o serviço de EC2;
- Clique em Load balancers;
- Clique em Create load balance;
- Selecione o Application Load Balancer(ALB);
- Preencha o nome do Load Balancer;
- No esquema selecione o <strong>internet-facing</strong>
- No <strong>Network mapping</strong> selecione a VPC desejada;
- Selecione 2 subnets: <strong>us-east-1a (use1-az2)</strong> e <strong>us-east-1b (use1-az4)</strong>
- Em <strong>Grupos de segurança</strong> selecione o Grupo de Segurança criado para o Balanceador de Carga.
- Em Listener <strong>HTTP:80</strong> no campo Ação padrão selecione o Grupo de Destino criado.
- Clique em Create load balancer.

## Criação da Instância
- Clique em instâncias, executar instância
### Nomes e Tags:
- Crie 3 Tags
<table>
    <tr>
      <th>Chave</th>
      <th>Valor</th>
    </tr>
    <tr>
      <td>Project</td>
      <td>PB</td>
    </tr>
    <tr>
      <td>CostCenter</td>
      <td>PBCompass</td>
    </tr>
    <tr>
      <td>Name</td>
      <td>Seu nome</td>
    </tr>
</table>

### Selecione a imagem de aplicação:
- Amazon Linux 2
### Selecione o tipo da instância:
- t3.small
### Selecionar/Criar o Key pair(login):
- Caso já tenha uma key pair não é necessario a criação de outra.
- Caso não possua, clique em Create new key pair, dê um nome a ela e faça o download do arquivo .pem ou .ppk(Putty)
### Edite as configurações de Rede:
- <strong>VPC: </strong>Selecione a VPC dejesada
- <strong>Sub-rede:</strong> Selecione a sub-rede criada
- Selecione um grupo de segurança existente(Default, siga o passo a passo no tópico: <strong>Alterando Grupo de Segurança</strong>)
### Configurar Armazenamento:
- Crie um disco GP2 de 8GiB
- Vá em detalhes avançados
- Copie o conteúdo do arquivo user_data.sh e cole em Dados do usuário
- Verifique todos os dados preenchidos e Clique em Executar Instância

## Caso seja necessario a criação de um novo Load Balancer
### Configurando o wp-config
- Como a montagem já é realizada através do user_data, faça a adição de duas novas linhas.
- Use: vim /nfs/antonio_durval/wordpress/wp-config.php
- Cole as seguintes linhas:
  <br><strong>define( 'WP_SITEURL', getenv_docker('WORDPRESS_SITEURL', 'localhost'));</strong><br>
  <strong>define( 'WP_HOME', getenv_docker('WORDPRESS_SITEURL', 'localhost'));</strong><br>
- Salve o arquivo
### Configurando o docker-compose.yml
- Vá até o repostiorio que foi clonado;
- Por exemplo: vim compass-atividade-docker/[docker-compose.yml](https://github.com/antonioDurval/compass-atividade-docker/blob/main/docker-compose.yml)
- Insira a seguite linha(Abaixo da linha 15(<strong>environment</strong> do serviço wordpress)):
  <br><strong>WORDPRESS_SITEURL: http://DNS_name do seu Load Balancer</strong>
- Após isso suba os conteiners novamente.
 
