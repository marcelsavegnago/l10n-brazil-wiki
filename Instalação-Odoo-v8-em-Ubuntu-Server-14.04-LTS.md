Neste tutorial será apresentado como fazer a instalação padrão do Odoo v8 pelo Github, com isso, teremos sempre as últimas versões liberadas pela comunidade. Antes de seguir os passos abaixo, sugerimos ter uma instalação limpa do Ubuntu, com as configurações de rede já definidas.


1. Instale o OpenSSH, que além de acesso ao servidor, permite também limitar potenciais ataques de força bruta:
	> sudo apt-get install openssh-server

2. Deve-se agora definir as configurações locais (Locale) do servidor. No terminal, execute os 4 comandos a seguir:
	> export LANGUAGE=pt_BR.UTF-8

	> export LANG=pt_BR.UTF-8

	> sudo locale-gen pt_BR pt_BR.UTF-8

	> sudo dpkg-reconfigure locales

_Caso esteja acessando o servidor via SSH, após os comandos acima, desconecte-se (logout) e conecte-se novamente (login)._

3. Ainda no terminal, atualize o seu sistema:
	> sudo apt-get update

	> sudo apt-get dist-upgrade


4. Crie o usuário “odoo” e que será o proprietário da aplicação e a sua respectiva pasta:
	> sudo adduser --system --home=/opt/odoo --group odoo


5. Instalar e configurar o serviço de banco de dados PostgreSQL:
	> sudo apt-get install postgresql


6. Deve-se agora configurar o usuário “odoo” no postgres, para isso, altere o usuário atual para postgres, a fim de ter os privilégios necessários para configurar a base de dados:
	> sudo su - postgres


7. Agora, crie o novo usuário do banco de dados. O usuário “odoo” terá direitos de acesso para se conectar, criar e eliminar bancos de dados. Anote a senha definida aqui, pois será necessário mais adiante:
	> createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo

	> Enter password for new role: ******* (Sua Senha para o usuário postgres 'odoo' )

	> Enter it again: *******


8. Saia do usuário postgres:
	> exit


9. Instalar git:
	> sudo apt-get install git



10. Instale as bibliotecas Python necessárias para o Odoo:
	> sudo apt-get install python-dev python-yaml python-feedparser python-geoip python-imaging python-pybabel python-unicodecsv wkhtmltopdf libxml2-dev libxmlsec1-dev python-argparse python-Babel python-cups python-dateutil python-decorator python-docutils python-feedparser python-gdata python-gevent python-greenlet python-Jinja2 python-libxslt1 python-lxml python-Mako python-MarkupSafe python-mock python-openid python-passlib python-psutil python-psycopg2 python-pychart python-pydot python-pyparsing python-pyPdf python-ldap python-yaml python-reportlab python-requests python-simplejson python-six python-tz python-unittest2 python-vatnumber python-vobject python-webdav python-Werkzeug python-wsgiref python-xlwt python-zsi python-dev libpq-dev poppler-utils python-pdftools antiword


11. Instale as bibliotecas Python necessárias para o Odoo:
	> sudo apt-get install python-pip   #Necessário para o PIP

	> sudo apt-get install python-setuptools   #Para Python v3 (python3-setuptools)

	> sudo pip install pyserial==2.7

	> sudo pip install psycogreen==1.0

	> sudo pip install pyusb==1.0.0b1

	> sudo pip install qrcode==5.0.1

	> sudo pip install Pillow==2.5.1

	> sudo pip install boto==2.38.0

	> sudo pip install oerplib==0.8.4

	> sudo pip install jcconv==0.2.3

	> sudo pip install pytz==2014.4


12. Instale as dependências do WKHTMLTOX:
	> sudo apt-get install xfonts-base xfonts-75dpi xfonts-encodings xfonts-utils fontconfig fontconfig-config libexpat1 libfontconfig1 libfreetype6 libjbig0 libjpeg8 libpng12-0 libssl1.0.0 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6 libxext6 libxfont1 libfontenc1 libxrender1 ttf-dejavu-core ucf x11-common


13. Altere para o usuário “odoo”. Com esse procedimento, vamos direto para a pasta /opt/odoo:
	> sudo su - odoo -s /bin/bash


14. Faça o download do corrente branch do odoo que está no github:
	> git clone https://www.github.com/odoo/odoo --depth 1 --branch 8.0 --single-branch .


15. Após finalizada a cópia, saia do usuário “odoo”:
	> exit


16. Deve-se agora criar um arquivo de configuração, baseado em um arquivo padrão e definir as respectivas permissões:
	> sudo cp /opt/odoo/debian/openerp-server.conf /etc/odoo-server.conf

	> sudo chown odoo: /etc/odoo-server.conf

	> sudo chmod 640 /etc/odoo-server.conf


17. Para permitir que o Odoo instale os módulos corretamente, deve-se alterar duas linhas nesse arquivo recém criado e adicionar uma terceira linha para o log. Use o seu editor de texto favorito: ex: sudo nano /etc/odoo-server.conf
	> Altere "db_password = False" para **“db_password = senha do postgres”** (passo 6).

	> Modifique a linha "addons_path = /usr/lib/python2.7/dist-packages/openerp/addons" para **"addons_path = /opt/odoo/addons"**

	> Adicione a seguinte linha: **logfile = /var/log/odoo/odoo-server.log**


_Mais adiante, voltaremos a editar esse arquivo para colocar o caminho da localização._


18. Crie a pasta para o arquivo de log e defina o respectivo proprietário:
	> sudo mkdir /var/log/odoo

	> sudo chown odoo:root /var/log/odoo


19. Criação do script de inicialização. Ao invés de utilizarmos o script padrão (/opt/odoo/debian/init), vamos utilizar um mais completo disponibilizado em http://www.comdesk.com.br/downloads/odoo/odoo-server.
	> cd /etc/init.d/   #Pasta padrão do Ubuntu dos scripts de inicalização

	> wget http://www.comdesk.com.br/downloads/odoo/odoo-server

	> sudo chmod 755 /etc/init.d/odoo-server   #Permissão para executar arquivo

	> sudo chown root: /etc/init.d/odoo-server   #Usuário root como proprietário do arquivo


20. Podemos agora testar o servidor. Para iniciar o servidor Odoo, digite:
	> sudo /etc/init.d/odoo-server start


21. É posssível também verificar o arquivo de log e conferir como o servidor foi iniciado:
	> cat /var/log/odoo/odoo-server.log

22. Se arquivo de log estiver ok, aponte o seu navegador de internet para o seguinte endereço:
	> http://IP_ou_domain:8069


23. Para seguir os pŕoximos passos, pare o servidor Odoo, digitando:
	> sudo /etc/init.d/odoo-server stop



Os próximos passos são necessários para a instalação da localização brasileira. Para facilitar o entendimento, vamos colocar os módulos na pasta /opt/odoo/localizacao.

24. Altere para o usuário “odoo”. Com esse procedimento, vamos direto para a pasta /opt/odoo:
	> sudo su - odoo -s /bin/bash


25. Crie uma pasta chamada “localizacao” e acesse-a:
	> mkdir localizacao

	> cd localizacao


26. Faça o download do branch 8.0 da localização que está no github:
	> git clone https://github.com/odoo-brazil/l10n-brazil.git --branch 8.0 --depth 1


27. Download do branch 8.0 do “Acoount Fiscal Rule”, que são dependências da localização:
	> git clone https://github.com/odoo-brazil/account-fiscal-rule.git --branch 8.0 --depth 1


28. Faça o download do branch 8.0 do “Eletronic Documents”, que são dependências da NFe:
	> git clone https://github.com/odoo-brazil/odoo-brazil-eletronic-documents.git --branch 8.0 --depth 1


29. Download do branch 8.0 do “Server Tools”, que são dependências, mantidas pela OCA:
	> git clone https://github.com/OCA/server-tools --branch 8.0 --depth 1


30. Após finalizado os downloads, saia do usuário “odoo”, retornando ao usuário padrão:
	> exit


31. Instalação do “Geraldo Reports”, necessário para relatórios pdf:
	> cd /tmp

	> git clone https://github.com/aricaldeira/geraldo --branch master

	> cd geraldo

	> sudo python setup.py install


32. Instalação do PySPED, necessário para NFe:
	> cd /tmp

	> git clone https://github.com/odoo-brazil/PySPED.git --branch 8.0

	> cd PySPED

	> sudo python setup.py install


33. Instalação do “pyxmlsec”, necessário para NFe:
	> cd /tmp

	> git clone https://github.com/aricaldeira/pyxmlsec --branch master

	> cd pyxmlsec

	> sudo python setup.py install


34. Após o download dos módulos, deve-se reconfigurar o arquivo de configurações. Use o seu editor de texto favorito: ex: sudo nano /etc/odoo-server.conf.
	> addons_path = /opt/odoo/addons,/opt/odoo/openerp/addons,/opt/odoo/localizacao/l10n-brazil,/opt/odoo/localizacao/account-fiscal-rule,/opt/odoo/localizacao/odoo-brazil-eletronic-documents,/opt/odoo/localizacao/server-tools

Caso tenha algum outro módulo, este deve ser indicado em “addons_path”.


35. Podemos agora testar o servidor. Para iniciar o servidor Odoo, digite:
	> sudo /etc/init.d/odoo-server start


36. É posssível também verificar o arquivo de log e conferir como o servidor foi iniciado:
	> cat /var/log/odoo/odoo-server.log


37. Se arquivo de log estiver ok, aponte o seu navegador de internet para o seguinte endereço:
	> http://IP_ou_domain:8069


38. Caso tenha funcionado corretamente, pode-se adicionar o script para que inicie automaticamente na inicialização do sistema:
	> sudo update-rc.d odoo-server defaults

_Agora é possível reiniciar o servidor que o Odoo iniciará automaticamente._


39. Ao digitar o comando a seguir:
	> ps aux | grep odoo


40. Você deve ver uma linha similar a mostrada abaixo, informando que o servidor está funcionando.
	> odoo  22743 0.2 9.3 147444 71392 ? Sl 01:30 0:05 python /opt/odoo/openerp-server -c /etc/odoo-server.conf


Nas próximas etapas, vamos criar o banco de dados e instalar os módulos necessários para a localização brasileira.

41. Acesso o Oddo pelo seu navegador de internet:
	> http://IP_ou_domain:8069

42. Começaremos configurando o banco de dados, portanto, a tela que aparece defina o seguinte:
	> *Senha da administração: mantenha a atual (admin)

	> *Selecione nome da base de dados: database_odoo (ex: comdesk_odoo)

	> *Carregar dados de demonstração: desmarcado

	> *Idioma predefinido: Português (BR)

	> *Escolha uma palavra-passe: Defina a sua senha

	> *Confirmar senha: Mesma escolhida na etapa anterior


43. Após a configuração do banco de dados, deve-se instalar os módulos básicos para o funcionamento do Odoo. Em “Configurações”, vá até “Módulos→Local Modules” e na listagem de módulos, instale os seguintes:

 	> CRM   *Módulo CRM padrão.

	> Gestão de Vendas   * Módulo de vendas padrão. Escolha o plano de contas Brasileiro, moeda e impostos básicos de compra e venda.

	> Online Biling   *Módulo padrão de transações on-line.

	> Contabilidade e Finanças   * Módulo contábil padrão.

	> Gestão de Armazém   #Módulo de estoque padrão.

	> Gestão de Compras   #Módulo de compras padrão.

_Demais módulos e dependências serão instalados automaticamente._


44. Continuando com a instalação, deve-se agora instalar os módulos desenvolvidos pela comunidade brasileira (Akretion, Danimar, Kmee, etc.). Ainda em “Módulos→Local Modules”, remova o item “Aplicativos” do campo de pesquisa e para facilitar a instalação, defina a “visualização em lista” e ordene por “autor”.


45. Na listagem de módulos locais, marque os seguintes módulos:

* Account Fiscal Position Rule Purchase
* Account Product Fiscal Classification
* Nota Fiscal Eletronica
* Account Fiscal Position Rule
* Account Fiscal Position Rule Sale
* Account Fiscal Position Rule Stock
* Scheduler Error Mailer
* Web Context Tunnel
* Brasileiro - Contabilidade
* Localização Brasileira - Módulo base contabilidade
* Localização Brasileira - Módulo de Pagamentos
* Localização Brasileira - Módulo Informações Contabéis do Produto
* Localização Brasileira - Módulo Contábil de Serviços
* Localização Brasileira - Módulo de Serviços
* Localização Brasileira - Módulo de Recibo de Pagamentos
* Localização Brasileira - Módulo Base
* Localização Brasileira - Módulo de CRM
* Localização Brasileira - Dados para o módulo de contabilidade
* Localização Brasileira - Dados do módulo de produtos
* Localização Brasileira - Dados do módulo de serviço
* Localização Brasileira - Dados do módulo Base
* Localização Brasileira - Módulo de Entrega
* Localização Brasileira - Módulo de Produtos
* Localização Brasileira - Módulo de compras
* Localização Brasileira - Módulo de Vendas
* Brazilian Localization Sale Product
* Brazilian Localization Sale Service
* Localização Brasileira - Módulo de Estoque
* Localização Brasileira - CEP
* Localização Brasileira - Módulo de Vendas e Estoque
* Manifesto Destinatário NFe



46. Após todos marcados, clique no botão [Mais], localizado na parte superior da tela de módulos locais e escolha a opção “Instale o módulo de imediato”.

A instalação de todos os módulos num mesmo momento, busca evitar o problema de dependências não instaladas.

**Se a instalação foi completada com sucesso, deve-se agora parametrizar o Odoo para que funcione de forma adequada às suas necessidades.**