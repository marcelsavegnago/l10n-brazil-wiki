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
 2. adicione uma linha com o produto 'Serviço', 
quantidade 2, preço unitário 10,00
 3. valide o pedido de venda
 
Resultado atual*:
 
 - Preço total de 10,00
 
Resultado esperado*:
 
 - Preço total de 20,00 (2 * 10 = 20)
```

*se puder adicionar imagens melhor


Contra qual versão eu devo submeter meu patch?
----------------------------------------------

Correções feitas nas branchs suportadas (atualmente **7.0**, **8.0** ) serão automaticamente transferidas paras o branchs superiores (develop). Não é necessário criar um Pull Request para a versão master se já foi criado para a versão 8.0 ou 7.0 por exemplo.

![Submitting against the right version](https://raw.githubusercontent.com/odoo/odoo/master/doc/_static/pull-request-version.png)

* **`l10n-brazil/develop`** para:
  * mudanças na API
  * mudanças que exijam modificação na estrutura do banco de dados (ex: novos campos) 
  * novas funcionalidades
* **`l10n-brazil/X.0`** (última versão estável, atualmente **7.0**) para:
  * correção de bugs que não violem a politica da versão estável (veja [abaixo](#what-does-stable-mean))


O que "estável" significa?
------------------------
Mudanças na versão estável devem respeitar estas orientações:
* Manter alterações ao mínimo: correções estáveis deve ter uma boa relação de valor / risco. Se o risco é muito alto ou um valor muito baixo, não devem ser incorporada na versão estável (devem ser na develop)
* Nenhuma "melhoria" (técnico ou funcional) deve ser feito na estável, eles apresentam uma relação de muito baixo valor / risco.
* Nenhuma mudança puramente cosmética (formatação, pep8, etc.)
* Nenhuma mudança na assinatura de métodos públicos em modelo (métodos não começam com um sublinhado)
* Não houve alteração do modelo de dados: armazenado colunas definições não devem ser alterados de maneiras incompatíveis sob quaisquer circunstâncias (sem adição / remoção / mudança tipo incompatível)
* Limitadas, alterações compatíveis, tais como a mudança `regras onDelete` ou` parâmetros size` são permitidos quando necessário.
* Para as mudança não-compatíveis, em casos extremos, um módulo de auto-instalar extra poderia ser adicionado a fim de corrigir automaticamente novas instalações sem quebrar os já existentes
* Nenhuma alteração das IDs XML de dados do módulo existente, não supressão de registos módulo que podem ser referenciados por dados do usuário em bancos de dados existentes, a menos que as mudanças são absolutamente essenciais e os registros estavam no modo "noupdate" inicialmente.
* Nenhuma mudança na estrutura dos fluxos de trabalho (novos / atividades realocados / transições), a menos que a mudança é 100% seguro para os registros existentes e não quebrar nada com ou sem atualização.
* Campos-não armazenados função pode ser adicionada, se for realmente necessário.
Arquivos XML (exibições, menus, dados padrão, etc.) só deve ser alterada se inevitável. Quando mudou, a mudança não deve ser obrigatória, ou seja, o código Python não deve depender da mudança.
* É bom se uma correção de bug requer uma atualização explícita, contanto que ele é seguro para usuários que não são conscientes disso e não realizar a atualização.
* Uma correção de erro não deve exigir a ctualização do código de fonte de 2 módulos diferentes, ao mesmo tempo, (ou servidor de complementos e ao mesmo tempo). A partir de v7 o sistema de atualização 1-click incorporado pode atualizar seletivamente módulos (incluindo a base), sem re-download a fonte de todos os módulos.
* Correções críticas de segurança não deve depender de uma atualização do módulo explícita para entrar em vigor, devem trabalhar com um simples pull+restart


Porque meu bug foi fechado sem efetuar o merge?
----------------------------------------
Um pull request é fechado quando ele não será incorporado na localização. Isto geralmente irá acontecer se a tarefa/bug:

* não é relevante para o desenvolvimento (label *invalid*)
* não é considerado um bug ou não se tem planos para mudar o comportamento atual (label *wontfix*)
* o bug já se encontra descrito em outro bug ou tarefa (label *duplicate*)
* o pull request deve ser resubmetido contra outra versão


Significado dos labels
-----------------

- **bloqueado**: uma correção ou informação do autor da requisição é requerida antes do merge
- **bug**: o bug foi confirmado
- **needs review**: um segundo nível de review é requerido
- **enhancement**: nova funcionalidade, deve ser discutido se será integrado ou desenvolvido
- **refactoring**: melhoramento de código
