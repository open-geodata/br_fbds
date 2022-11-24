# Fundação Brasileira para o Desenvolvimento Sustentável - FBDS

- [GitHub](https://github.com/open-geodata/br_fbds)
 
<br>

Em novembro de 2022 surgiu a necessidade/curiosidade de melhor compreender os dados de hidrologia e uso do solo disponiibilizados pela [FBDS](https://www.fbds.org.br). Os dados são disponibilizados em um [**repositório público de mapas e _shapefiles_ para _download_**](https://geo.fbds.org.br/).

Abaixo segue informações obtidas no _site_ da Fundação:

> Em 2015 a [Fundação Brasileira para o Desenvolvimento Sustentável](https://www.fbds.org.br) deu início ao Projeto de Mapeamento em Alta Resolução dos Biomas Brasileiros, que desde então vem produzindo dados primários de uso e cobertura do solo, hidrografia e Áreas de Preservação Permanente em uma resolução inédita para os biomas brasileiros (5 metros).

> Os resultados do mapeamento vêm sendo utilizados para apoiar a execução de políticas públicas - em especial a implementação do Cadastro Ambiental Rural, o planejamento territorial, a realização de pesquisas acadêmicas e o desenvolvimento de tecnologias. Até o momento o mapeamento já foi concluído para mais de 4 mil municípios brasileiros abrangidos pelos biomas Mata Atlântica e Cerrado.

<br>

---

### _Scrapy_

A interface do repositório foi construída em com o _framework_ [**_h5ai_**](https://larsjung.de/h5ai) que, se assemelha, a estrutura de um servidor FTP.

A ideia então utilizada foi obter a lista dos arquivos para, posteriormente, fazer o _download_. Para criar a lista de arquivos, fiz com auxílio do _selenium_. Com o _driver_ eram realizados os seguintes procedimentos:

1. Listar todos as **Subpastas** e **Arquivos** de um diretório raiz;
2. Para cada **Subpasta** encontrada, entra-se nela, e repetir o procedimento de listar **Arquivos**), retornando para a pasta anterior ao final
3. Fazia isso de modo em _loop_, utilizando uma função recursiva.

<br>

Pesquisando sobre APIs para Com falha utilizar o _framework_ [**_h5ai_**](https://larsjung.de/h5ai), encontrei insformação sobre um _bug_, no [Exploit-DB.com](https://www.exploit-db.com/exploits/38256)

<br>

Uma vez com todos os links, foi realizado o download usando o JDownloder.

<br>

---

### _TODO_

1. Definir funções de fazer o _download_ dos arquivos, a partir da lista.
2. Realizar o agendamento para obter a lista.






https://geo.fbds.org.br/Metadados.pdf
https://geo.fbds.org.br/Metadados.xml
https://geo.fbds.org.br/Metodologia.pdf

SP_BARRA%20_DO_TURVO_APP