Reportando Bugs
----------------

Se possível, sempre adicione um Pull Request quando criar uma tarefa ou bug (Github vai criar automaticamente um "issue" quando submeter as modificações).

Se depois você criar um Pull Request resolvendo o problema, não se esqueça de referenciar o problema original no seu Pull Request (ex: "Este patch corrige o problema #42").

Quando reportar um bug ou criar um Pull Request, por favor use a seguinte estrutura:

```
**Campo quantidade é ignorado em um pedido de venda**

Versões com problema:
 
 - 7.0 e acima
 
Passos para reproduzir:
 
 1. criar um novo pedido de venda
 2. adicione uma linha com o produto 'Serviço', quantidade 2, preço unitário 10,00
 3. valide o pedido de venda
 
Resultado atual*:
 
 - Preço total de 10,00
 
Resultado esperado*:
 
 - Preço total de 20,00 (2 * 10 = 20)
```

*se puder adicionar imagens melhor


Contra qual versão eu devo submeter meu patch?
----------------------------------------------

Correções feitas nas branchs suportadas (atualmente **7.0**, **8.0** ) serão automaticamente transferidas paras o branchs superiores (master). Não é necessário criar um Pull Request para a versão master se já foi criado para a versão 8.0 ou 7.0 por exemplo.

![Submitting against the right version](https://raw.githubusercontent.com/odoo/odoo/master/doc/_static/pull-request-version.png)

* **`l10n-brazil/develop`** para:
  * mudanças na API
  * mudanças que exijam modificação na estrutura do banco de dados (ex: novos campos) 
  * novas funcionalidades
* **`l10n-brazil/X.0`** (última versão estável, atualmente **7.0**) para:
  * correção de bugs que não violem a politica da versão estável (veja [abaixo](#what-does-stable-mean))


O que "estável" significa?
------------------------
Changes in stable series must respect these guidelines:
* Keep changes to a minimum: stable patches must have a good value/risk ratio. If risk is too high or value too low, it must not be merged at all in stable (rather in the development series)
* No "improvement" (technical or functional) should be done in stable, they typically have a very low value/risk ratio. Often, the functional coverage is voluntarily limited.
* No purely cosmetic changes (formatting, pep8, etc.)
* No change in the signature of public methods on model (methods not starting with an underscore)
* No data model change: stored columns definitions must not be altered in incompatible manners under any circumstances (no addition / removal / incompatible type change)
* Limited, compatible changes such as changing `ondelete` rules or `size` parameters are allowed when necessary.
* For non-compatible change, in extreme cases an extra auto-install module could be added in order to automatically patch new installations without breaking existing ones
* No change to the XML IDs of existing module data, and no deletion of module data records that may be referenced by user data in existing databases, unless the changes are absolutely essential and the records were in "noupdate" mode initially.
* No change in the structure of workflows (new/relocated activities/transitions) unless the change is 100% safe for existing records and does not break anything with or without update.
* Non-stored function fields may be added if it is really necessary.
XML files (views, menus, default data, etc.) should only be changed if inevitable. When changed, the change must not be mandatory, i.e. the Python code must not depend on the change.
* It is fine if a bugfix requires an explicit update, as long as it is safe for users who are not aware of it and do not perform the update.
* A bugfix must not require updating the source code of 2 different modules at the same time, (or server and addons at the same time). As of v7 the embedded 1-click update system may selectively update modules (including base) without re-downloading the source of all modules.
* Critical security fixes must not depend on an explicit module update to take effect, they must work with a simple pull + restart


Porque meu bug foi fechado sem efetuar o merge?
----------------------------------------
Um pull request é fechado quando ele não será incorporado na localização. Isto geralmente irá acontecer se a tarefa/bug:

* não é relevante para o desenvolvimento (label *inválido*)
* não é considerado um bug ou não se tem planos para mudar o comportamento atual (label *semcorrecao*)
* o bug já se encontra descrito em outro bug ou tarefa (label *duplicado*)
* o pull request deve ser resubmetido contra outra versão


Significado dos labels
-----------------

- **blocked**: a fix or information from the author of the request is required before merging
- **confirmed**: issue was validated by qualification team
- **need review**: a second level of review is required
- **wishlist**: new feature, to discuss if will be integrated
- **RDWIP**: Internal pull request, work in progress (by odoo SA RD team)
- **OE**: pull request created to solve an odoo enterprise ticket
