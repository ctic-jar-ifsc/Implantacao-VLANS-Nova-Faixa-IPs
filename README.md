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

Outra forma de controle da rede é a adoção de IPs públicos para cada dispositivo ao invés dos IPs privados. Desta forma, se algum computador estiver enviando SPAM por estar infectada com vírus por exemplo, fica muito mais fácil e rápido localizá-lo e desta forma contornar o problema. Por isso, será utilizada a nova faixa de IPs públicos fornecida pela RNP nos computadores da instituição. 

Por fim, com o recente esgotamento dos IPs públicos no mundo todo, será inevitável a utilização dos endereços IP no novo formato através do protocolo IPv6. O endereçamento IPv6 permite redes gigantescas, acabando com problemas de esgotamento de IPs por muitos anos. Assim, pretendemos também implementar a comunicação IPv6 concomitantemente com a IPv4, estando então preparados para o presente e futuro.

## Fases do Projeto 
* Elaboração dos modelos de Documentação da Infraestrutura de Redes do Campus
* Definição das novas Subredes e VLANs de acordo com os _512_ IPs públicos disponíveis, além das Faixas IPv6 (Este sem limite de IPs)
* Documentação dos Racks, Switchs e Patch Panels do Campus
* Implementação da nova configuração em Servidor Firewall PfSense e nos equipamentos Switch Gerenciáveis
* Testes e Atualizações de Documentação



## (14/08/2018) - Implantação 

Após o término da implantação da InfraEstrutura física por parte da empresa responsável, passamos a efetuar um pente fino, a fim de organizar o cabeamento tanto do Datacenter / CPD, como de alguns Racks de Distribuição, no intuito de facilitar a identificação dos pontos durante a implantação da nova Rede IP, a configuração dos equipamentos switch e das manutenções futuras

Em seguida, passamos para a definição da nova rede lógica (Novo Bloco de IPs da RNP), através da escolha das novas Subredes e VLANs de acordo com os 512 IPs públicos disponíveis, além das Faixas IPv6 (Esta sem a limitação de quantidade de IPs disponíveis).

Após esta definição, passamos a realizar a configuração de todos os cerca de 20 equipamentos  Switch Gerenciáveis, responsáveis por concentrar o cabeamento estruturado do campus, onde esta configuração inclui:
- definição de usuários/senhas de acesso através de SSH / Telnet seguro
- criação das VLANs definidas anteriormente
- definição de portas tronco de forma padronizada
- documentação através de planílha específica
 
A implementação do ambiente foi iniciada com a instação e configuração do Switch Camada 3 em modo Roteador, sendo ele responsável por todo o roteamento de VLANS, Filtro de Pacotes através de ACL, DHCP-Relay, etc, ou seja, ele passa a ser o coração da rede neste modelo. 
Também instalamos um novo servidor Firewall PFsense, sendo este responsável apenas pelo filtro dos pacotes que entram e saem para a Internet, através do link direto com nosso roteador de saída e com a rede interna (Switch Camada 3).

Com tudo isto feito, finalmente passamos a fase de implantação. Neste primeiro momento fizemos a migração em dois ambientes:
- TI: Temos uma VLAN específica para estes servidores, uma vez que precisamos de acessos exclusivos à manutenção dos servidores de redes entre outros.
- Laboratório A1: Primeiro laboratório em fase de testes com a nova rede

A migração agora ocorrerá aos poucos, assim, evitaremos maiores transtornos e ela tenderá a ocorrer de forma transparente pelos usuários


Com relação à documentação dos Racks, Switchs e Patch Panels do Campus, ainda estamos definindo quais padrões utilizaremos. Algumas propostas incluem o uso de CorelDraw para desenho dos equipamentos, ou então de alguma ferramenta mais específica para este uso como o Microsoft Visio.

Por hora, estamos atualizando a descrição das próprias portas dos Switches para a idetificação dos pontos.


