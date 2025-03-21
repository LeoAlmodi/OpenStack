## Objetivo

- Entender os conceitos básicos sobre uma plataforma de gerenciamento de hardware e introduzir conceitos básicos sobre redes de computadores.


### Instalando o MAAS

<!-- termynal -->

``` bash
sudo snap install maas --channel=3.5/Stable
```

![Tela do Dashboard do MAAS](./maas.png)
/// caption
Dashboard do MAAS.
///

Conforme ilustrado acima, a tela inicial do MAAS apresenta um dashboard com informações sobre o estado atual dos servidores gerenciados. O dashboard é composto por diversos painéis, cada um exibindo informações sobre um aspecto específico do ambiente gerenciado. Os painéis podem ser configurados e personalizados de acordo com as necessidades do usuário.

### Configurando o MAAS

Inicializando o MaaS:
<!-- termynal -->

``` bash
sudo maas init region+rack --maas-url http://172.16.0.3:5240/MAAS --database-uri maas-test-db:///
sudo maas createadmin
```

Gerando um par de chaves para autenticação.
<!-- termynal -->

``` bash
ssh-keygen -t rsa
cat ./.ssh/id_rsa.pub
```

#### Chaveando o DHCP

Foi habilitado o DHCP na subrede pelo MaaS Controller, e então, alterado o Reserved Range.

Após isso, o DNS da subnet foi apontado para o DNS da rede externa, e por fim, desabilitado o DHCP do roteador.


#### Checando saúde do MAAS

Confirmamos a saúde do sistema MAAS visitando a página Controladores na interface web (dashboard).

#### Comissionando servidores 

Nessa etapa, cadastramos os hosts (machines) disponíveis, server 1 até server 5. 

Para cada máquina, fizemos a seguinte etapa.

- Alteramos a opção *Power Type* para *Intel AMT*
- Colocamos o MacAddress da máquina
- Colocamos a senha 
- Por fim, o IP do AMT.

E no final, também adicionamos o Roteador como devices no dashboard do MaaS.

#### Criando OVS bridge

Uma Open vSwitch (OVS) bridge reduz a necessidade de duas interfaces de rede físicas.

O nome da ponte será referenciado em outras partes dos roteiros, chamaremos de "br-ex".

Será feito isso para todos os hosts.



## App



### Tarefa 1

Após instalar o postgresql no server 1, iremos testar algumas coisas.

![Tarefa 1 - 1](./img/R1_T1_1.png)
/// caption
Funcionando e seu Status está como "Ativo" para o Sistema Operacional.
///
![Tarefa 1 - 2](./img/R1_T1_2.png)
/// caption
Acessivel na própria maquina na qual ele foi implantado.
///
![Tarefa 1 - 3](./img/R1_T1_3.png)
/// caption
Acessivel a partir de uma conexão vinda da máquina MAIN.
/// 
![Tarefa 1 - 4](./img/R1_T1_4.png)
/// caption
Verfificando em qual porta este serviço está funcionando.
///

### Tarefa 2

Agora iremos instalar no server 2 o Django manualmente.

![Tarefa 2 - 1](./img/R1_T2_1.png)
/// caption
Dashboard do **MAAS** com as máquinas.
///

![Tarefa 2 - 2](./img/R1_T2_2.png)
/// caption
Aba images, com as imagens sincronizadas.
///

![Tarefa 2 - 3_s1](./img/R1_T2_3_s1.png)
/// caption
Server 1 - testes de hardware e commissioning com Status "OK".
///

![Tarefa 2 - 3_s2](./img/R1_T2_3_s2.png)
/// caption
Server 2 - testes de hardware e commissioning com Status "OK".
///

![Tarefa 2 - 3_s3](./img/R1_T2_3_s3.png)
/// caption
Server 3 - testes de hardware e commissioning com Status "OK".
///

![Tarefa 2 - 3_s4](./img/R1_T2_3_s4.png)
/// caption
Server 4 - testes de hardware e commissioning com Status "OK".
///

![Tarefa 2 - 3_s5](./img/R1_T2_3_s5.png)
/// caption
Server 5 - testes de hardware e commissioning com Status "OK".
///

### Tarefa 3

![Tarefa 3 - 1](./img/R1_T3_1.png)
/// caption
Aqui está o Django do servidor 2 funcionando.
///

![Tarefa 3 - 2](./img/R1_T3_2.png)
/// caption
Aplicação Django conectada ao server.
///


Primeiramente, duas máquinas (server 1 e server 2) foram alocadas, e em seguida o grupo deu deploy em ambas com a imagem do ubuntu 22.04 LTS.
Após isso, dentro do server 1, foi instalado o postgreSQL, criado um usuario para a aplicação, e, então, criado um database chamado tasks. Depois disso, dois arquivos foram mudados (postgresql.conf e pg_hba.conf), para expor o serviço para acesso e deixar liberado para qualquer máquina dentro da subnet do kit. No fim, foi liberado o firewall e reinicidado o serviço.
No server 2, foi clonado um repositório de instalação do django, e foi rodado o script de instalação. Após a instalação foi configurado que o banco de dados seja o criado no server 1. Logo depois, foi dado reboot do server, adicionado o IPV4 do server1 no /etc/hosts e testado o acesso do serviço pela porta 8080 no terminal do MaaS.

### Tarefa 4

Também foi instalado no server 3 a aplicação Django, porém utilizando a ferramenta Ansible.

![Tarefa 4 - 1](./img/R1_T4_1.png)
/// caption
Dashboard do MAAS com as 3 Maquinas e seus respectivos IPs.
///

![Tarefa 4 - 2](./img/R1_T4_2.png)
/// caption
Aplicação Django conectada ao server 2.
///

![Tarefa 4 - 3](./img/R1_T4_3.png)
/// caption
Aplicação Django conectada ao server 3.
///

A diferença entre instalar o Django manualmente e utilizando o Ansible está na forma como os comandos são executados. Na instalação manual, cada comando precisa ser digitado e executado individualmente, tornando o processo mais demorado e suscetível a erros. Já com o Ansible, todas as etapas da instalação são definidas em um playbook YAML, permitindo que a configuração seja aplicada automaticamente e de forma padronizada. Isso torna o processo mais eficiente, reprodutível e escalável, especialmente em ambientes com múltiplos servidores.

### Tarefa 5

Para montar o ponto único de entrada, utilizaremos uma aplicação de proxy reverso como load balancer. Será instalado no server 4.

![Tarefa 5 - 1](./img/R1_T5_1.png)
/// caption
Dashboard do MAAS com as 4 Maquinas e seus respectivos IPs.
///

![Tarefa 5 - 2_s2](./img/R1_T5_server2.png)
/// caption
Pelo load balancer, o server 2 foi escolhido.
///

![Tarefa 5 - 2_s3](./img/R1_T5_server3.png)
/// caption
Pelo load balancer, o server 3 foi escolhido.
///


## Discussões


As principais difuculdades encontradas foram quando encontrávamos algum erro de permissão, ou algo parecido, era difícil entender aonde encontrar o arquivo e arrumá-lo. Porém o que foi mais fácil foi entender, em geral, o uso e a importância do MaaS para a subrede. E o mais difícil foi fazer o load balancer.

## Conclusão

O que foi possível concluir com a realização do roteiro?

Em conclusão, o roteiro foi importante para o aprendizado e entendimento do bare metal e o básico da criação de uma subrede. Também foi interessante entender alguns problemas comuns na criação, como por exemplo a questão de permissão e conexão entre os hosts.
