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


