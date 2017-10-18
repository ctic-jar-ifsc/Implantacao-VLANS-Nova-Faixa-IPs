# Maior Organização, Controle, Segurança e Desempenho e Pronto para o Futuro

Neste repositório é descrito o projeto de migração e implantação da nova faixa de IPs públicos
da RNP e também da segregação da rede atual em várias redes locais virtuais ou Virtual LANs (VLANs),
além da documentação e implementação do mesmo ambiente em no formato IPv6, em substituição ao atual 
modelo em que todos os computadores e dispositivos encontram-se em uma única rede de IPs internos.

Para isso, serão utilizados documentos de base internos (normativas do IFSC) e externos (trabalhos 
acadêmicos e demais documentações e boas práticas encontradas na internet).

## Porque migrar para um ambiente mais complexo?
A medida que as organizações crescem, também aumenta a necessidade de maior organização e controle da infraestrutura e dos serviços de rede.

Um problema comum apresentado em uma rede única é o grande número de pacotes broadcast (pacotes destinados a todos os dispositivos) que em demasia provoca lentidão no acesso à rede. No momento em uma rede é divida em redes menores, os pacotes de broadcast também só serão transmitidos dentro destas sub-redes, diminuindo consideravelmente o problema.

Além disso, a subdivisão em redes menores permite maior controle, facilitando identificar o IP de origem de determinado problema. Um exemplo é colocando todos os computadores de um laboratório de informática dentro de uma VLAN específica.

Outra forma de controle da rede é a adoção de IPs públicos para cada dispositivo ao invés dos IPs privados. Desta forma, se algum computador estiver enviando SPAM por estar infectada com vírus por exemplo, fica muito mais fácil e rápido localizá-lo e assim contornar o problema. Por isso, será utilizada a nova faixa de IPs públicos fornecida pela RNP nos computadores da instituição. 

Por fim, com o recente esgotamento dos IPs públicos no mundo todo, será inevitável a utilização dos endereços IP no novo formato através do protocolo IPv6. Assim, pretendemos também implementar a comunicação IPv6 concomitantemente com a IPv4, estando assim preparado para o presente e futuro

--------------------------------





* Fluxo natural da tecnologia.
* Economia de recurso/_hardware_.
* Facilidade de agregar e manter novos serviços.
* Centralização de gerência, monitoramento, administração etc.
* Serviços encapsulados podem ser movidos mais facilmente
local <=> nuvem privada <=> nuvem pública.
* Controle de versão + moderação + automação de testes = CI/CD.
* Alta disponibilidade, fácil, fácil.
* Auto escalonamento, fácil, fácil.
* Google usa há 15 anos em todos os seus serviços, bilhões de contêineres por
semana. Se eles podem, nós também pode(re)mos.


## Estrutura do projeto macro
![Projeto Macro](docs/projeto_macro_ctic.jpg)


## Serviços que podemos/queremos oferecer
![Projeto Macro](docs/servicos_possiveis.png)

Nesse repositório estamos colocando cada implementação desenvolvida.
Estamos utilizando a seguinte estratégia de atuação:
* Migrar/instalar serviços já implementados em `docker compose` ou Kubernetes.
* Migrar serviços não críticos para testar a tecnologia.
* Implementações novas importantes e críticas como fase de testes/migração.
* Implementar serviços internos para testes de estabilidade e desempenho da
tecnologia.

## Armazenamento de estados e dados

Utilizamos uma abordagem para armazenamento de estados e dados onde esses não
são salvos na mesma estrutura onde roda o Kubernetes, e sim em uma estrutura de
armazenamento centralizada. Os
[pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) montam um
armazenamento NFS e utilizam.
A implementação do storage pode ser encontrada em
[cticsjeifsc/storage](https://github.com/ctic-sje-ifsc/storage).

Podemos ver um exemplo a seguir de um
[volume persistente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
(PV):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: netbox-postgresql-base
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: storage1
    path: /mnt/storage/storage/kubernetes/ifsc/sje/a/saas/srv/netbox/postgresql/base
```

# Serviços Web

## _Front-end_ Nginx

A proposta básica é oferecer os serviços preferencialmente na modalidade
[SaaS](https://pt.wikipedia.org/wiki/Software_como_servi%C3%A7o) com:
1. Disponibilidade: uso de vários servidores físicos e/ou virtuais para manter
os serviços operando sem sobressaltos.
1. Segurança: uso exclusivo de transmissão criptografada, como
[HTTPS](https://www.ssllabs.com/ssltest/analyze.html?d=projetos.sj.ifsc.edu.br&latest),
WebSocket sobre TLS, SSH e outros.
1. Desempenho: [HTTP/2](https://http2.github.io/), balanceamento de carga, cache
HTTP etc.

A grande maioria dos serviços aqui listados são baseados em HTTP.
Para garantir que todos esses atenderão aos requisitos anteriores
(independente de serem projetados para tal), existe um _front-end_ distribuído
(e escalável), o
[srv/nginx](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/nginx),
de forma a padronizar o acesso.


## [Netbox](https://netbox.sj.ifsc.edu.br)

O primeiro serviço migrado foi o [netbox](https://netbox.sj.ifsc.edu.br/), que já estava rodando em container em uma VM. Pode ser encontrado em [srv/netbox](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/netbox).


## [Sharelatex](https://sharelatex.sj.ifsc.edu.br)

Implementamos o [sharelatex](https://sharelatex.sj.ifsc.edu.br/).
A implementação está em
[srv/sharelatex](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/sharelatex).


## [Rocket.Chat](https://chat.sj.ifsc.edu.br)

Finalizamos a implementação do [Rocket.chat](https://chat.sj.ifsc.edu.br/) e utilizaremos ele
em substituição do Slack, assim poderemos estar testando a estabilidade do kubernetes.
O acesso é feito com o usuário do LDAP, tanto para alunos quando para Servidores. Implementação [srv/rocketchat](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/rocketchat).

## [Nextcloud](https://nextcloud.sj.ifsc.edu.br)

O [Nextcloud](https://nextcloud.com) é um serviço Open Source de armazenamento e
sincronização de arquivos privados, similar ao Dropbox (proprietário).
O acesso é feito com o usuário do LDAP, tanto para alunos quando para Servidores.
Implementação do Nextcloud [srv/nextcloud](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/nextcloud).

## [Onlyoffice](https://nextcloud.sj.ifsc.edu.br)

O [Onlyoffice Document Server](http://onlyoffice.org/) é uma suíte de escritório on-line composta de visualizadores e editores de textos, planilhas e apresentações, totalmente compatível com os formatos Office Open XML: .docx, .xlsx, .pptx e habilitando a edição colaborativa em tempo real. O acesso ao Onlyoffice Document Server é feito pelo [Nextcloud](https://nextcloud.sj.ifsc.edu.br). Implementação do OnlyofficeDocumentServer [srv/onlyoffice](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/onlyoffice).


## [Wordpress](https://wordpress.sj.ifsc.edu.br)

[WordPress](https://br.wordpress.org) é um aplicativo de sistema de
gerenciamento de conteúdo para web, escrito em PHP com banco de dados MySQL,
voltado principalmente para a criação de sites e blogs via web.  Implementação
do Wordpress
[srv/wordpress](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/wordpress).


# Serviços baseados em SSH

## OpenLDAP

Utilizada a imagem
[openldap](https://github.com/ctic-sje-ifsc/imagens/tree/master/openldap)
para base de usuários.

[![](https://images.microbadger.com/badges/image/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own license badge on microbadger.com")

## Matlab

[MATLAB](https://www.mathworks.com/products/matlab.html) trata-se de um software interativo de alta performance voltado para o cálculo numérico.
Utilizada a imagem
[matlab](https://github.com/ctic-sje-ifsc/imagens/tree/master/matlab)
para execução remota da aplicação via SSH.

```sh
ssh -XC seulogin@matlab.sj.ifsc.edu.br -p 2222
```

[![](https://images.microbadger.com/badges/image/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own license badge on microbadger.com")

## Octave

[GNU Octave](https://www.gnu.org) é uma linguagem de alto nível, principalmente destinada a computação numérica. Ele fornece uma conveniente interface de linha de comando para resolver problemas numericamente lineares e não-lineares.
Utilizada a imagem
[octave](https://github.com/ctic-sje-ifsc/imagens/tree/master/octave)
para execução remota da aplicação via SSH.

```sh
ssh -XC seulogin@octave.sj.ifsc.edu.br -p 2223
```

[![](https://images.microbadger.com/badges/image/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own license badge on microbadger.com")

## Nyqlab

[NyqLab](https://github.com/rwnobrega/nyqlab) é um software educacional que visa ajudar os alunos a aprender e praticar conceitos básicos em sistemas de comunicação analógicos e digitais.
Utilizada a imagem
[nyqlab](https://github.com/ctic-sje-ifsc/imagens/tree/master/nyqlab)
para execução remota da aplicação via SSH.

```sh
ssh -XC seulogin@nyqlab.sj.ifsc.edu.br -p 2225
```

[![](https://images.microbadger.com/badges/image/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own license badge on microbadger.com")


# Outros Serviços

## Mosquitto

Utilizada a implementação [Mosquitto](https://mosquitto.org/) para MQTT _Broker_.

Para o serviço, que por enquanto opera apenas com protocolos MQTT v3.1 e v3.1.1,
foi criada uma imagem
[mosquitto](https://github.com/ctic-sje-ifsc/imagens/tree/master/mosquitto).

[![](https://images.microbadger.com/badges/image/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own license badge on microbadger.com")
