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
