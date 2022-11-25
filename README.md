# Fundação Brasileira para o Desenvolvimento Sustentável - FBDS

- [GitHub](https://github.com/open-geodata/br_fbds)

<br>

Em novembro de 2022 surgiu a necessidade/curiosidade de melhor compreender os dados de hidrologia e uso do solo disponiibilizados pela [Fundação Brasileira para o Desenvolvimento Sustentável (FBDS)](https://www.fbds.org.br). Os dados são disponibilizados em um [**repositório público de mapas e _shapefiles_ para _download_**](https://geo.fbds.org.br/).

Abaixo segue informações obtidas no _site_ da Fundação:

> Em 2015 a [Fundação Brasileira para o Desenvolvimento Sustentável](https://www.fbds.org.br) deu início ao Projeto de Mapeamento em Alta Resolução dos Biomas Brasileiros, que desde então vem produzindo dados primários de uso e cobertura do solo, hidrografia e Áreas de Preservação Permanente em uma resolução inédita para os biomas brasileiros (5 metros).

> Os resultados do mapeamento vêm sendo utilizados para apoiar a execução de políticas públicas - em especial a implementação do Cadastro Ambiental Rural, o planejamento territorial, a realização de pesquisas acadêmicas e o desenvolvimento de tecnologias. Até o momento o mapeamento já foi concluído para mais de 4 mil municípios brasileiros abrangidos pelos biomas Mata Atlântica e Cerrado.

<br>

---

## _h5ai_

A interface do repositório foi construída em com o _framework_ [**_h5ai_**](https://larsjung.de/h5ai) que, se assemelha, a estrutura de um servidor FTP. Foi importante estudar o funcionamento do _framework_, do lado do cliente, para descobrir melhores maneiras de "raspar" os dados.

Pesquisei a possibilidade de existirem APis em python para "raspar", de modo facilitado, os dados. Na ausência de APIs, foi necessário pensar em técnicas de _webscrapping_.

Pesquisando sobre APIs para Com falha utilizar o _framework_ [**_h5ai_**](https://larsjung.de/h5ai), encontrei insformação sobre um _bug_, no [Exploit-DB.com](https://www.exploit-db.com/exploits/38256)

<br>

---

## Abordagens

### Primeira Abordagem (_ruim e, portanto, descontinuada_)

A ideia então utilizada foi obter a lista dos arquivos para, posteriormente, fazer o _download_.

Usando o _./scripts_/**01_get_data.ipynb**, foi utilizada a seguinte concepção: Para criar a lista de arquivos, fiz com auxílio do _selenium_. Com o _driver_ eram realizados os seguintes procedimentos:

1. Listar todos as **Subpastas** e **Arquivos** de um diretório raiz;
2. Para cada **Subpasta** encontrada, entra-se nela, e repetir o procedimento de listar **Arquivos**), retornando para a pasta anterior ao final
3. Fazia isso de modo em _loop_, utilizando uma função recursiva.
4. A cada iteração, todas as URLs apresentadas eram colecionadas em um tabela _.csv_.

<br>

![Abordagem_1](docs/imgs/Peek%202022-11-24%2021-49.gifPeek 2022-11-24 21-49.gif)

<br>

Uma vez com todos os links, foi realizado o download usando o JDownloder, com arquivo _./scripts_/**03_download_list_files.ipynb**

<br>

---

### Segunda Abordagem (_melhor!!_)

A partir do [diretório do Estado de São Paulo](https://geo.fbds.org.br/SP/), com 645 pastas (uma para cada município), foi realizado o _download_ da pasta, resultando em 645 arquivos _.tar_. Isso foi feito com o arquivo _./scripts_/**01_get_data.ipynb**. A ideia era:

1. Listar todos as **Subpastas** (que represetam os municípios) de um diretório raiz;
2. Usando os conceitos de [_ActionChains_](https://www.selenium.dev/selenium/docs/api/py/webdriver/selenium.webdriver.common.action_chains.html), passar o mouse sobre a pasta e clicar nela.
3. Clicar no botão "Fazer Download".

<br>

![Abordagem_2](docs/imgs/Peek%202022-11-24%2021-49.gifPeek 2022-11-24 21-53.gif)

<br>

Após isso, com uso do _scripts_/**02_adjust_data.ipynb**, foram feitos os seguintes procedimentos:

1. Listar todos os arquivos _shapefile (.shp)_ que estão dentro dos arquivos _.tar_, sem descompactar!
2. Criar uma tabela com essa lista de arquivos.
3. Ajustar essa tabela, agregando diversas informações para re-criar os caminhos para o arquivo.
4. Criar cententas de arquivos temporários (com auxílio da bibliotenca [_tempfile_](https://docs.python.org/3/library/tempfile.html)), com o mesmo padrão de nome, evitando a necessidade de descompactar os arquivos _.tar_
5. Listar os arquivos _fake shapefile_ e ajusta-los, para que direcionem ao arquivo dentro do _.tar_
6. Testar e leitura dos arquivos pelo geopandas, conectar e salvar... para cada feição.

<br>

---

### _TODO_

1. ~~Definir funções de fazer o _download_ dos arquivos, a partir da lista.~~
2. ~~Realizar o agendamento para obter a lista.~~
3. ~~Ajustar a pasta de _download_. Atualmente vai para a pasta padrão. Movi manualmente!~~
4. Ajustar os tipos de arquivos (Pontos, Polylines, Polygons), visto que na lista de arquivos surgiu uma feição curiosa:
   1. _RIOS_DUPLOS.shp_
   2. _RIOS*DUPLOS*.shp_
   3. _RIOS_DUPLOS_POL.shp_
