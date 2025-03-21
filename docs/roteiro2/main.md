## Objetivo

- Entender os conceitos básicos sobre uma plataforma de gerenciamento de aplicações distribuídas.
- Entender os conceitos básicos de comunicação entre aplicações e serviços.

#### Pré-requisitos

- Ter o Bare Metal feito (roteiro anterior)

## Criando Infraestrutura para deploy com Juju

Para montar a nossa Cloud Privada, vamos utilizar um orquestrador de deploy que integra com o MaaS. Primeiramente, temos que verificar 
se todas as máquinas estão ready.

Após isso, o Juju deve ser instalado no main:

<!-- termynal -->

``` bash
sudo snap install juju --channel 3.6
```

Com o Juju instalado, devemos criar dois arquivos:

- Criar um arquivo de configuração _maas-cloud.yaml_, e depois de salvar, rodar o seguinte comando:
    <!-- termynal -->
``` bash
$ juju add-cloud --client -f maas-cloud.yaml maas-one
```

- Criar um arquivo de credenciais _maas-creds.yaml_, e depois de salvar, rodar o seguinte comando:
    <!-- termynal -->
``` bash
$ juju add-credential --client -f maas-creds.yaml maas-one
```

Iremos criar o controlador no server1 para isso faça a tag da maquina com o nome juju através do dashboard MAAS na maquina server1.

<!-- termynal -->
``` bash
juju bootstrap --bootstrap-series=jammy --constraints tags=juju maas-one maas-controller
```


## App






