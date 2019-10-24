Nas versões 12+ você ira encontrar 3 arquivos na raiz do repositório:

https://github.com/OCA/l10n-brazil/blob/12.0/.pre-commit-config.yaml
https://github.com/OCA/l10n-brazil/blob/12.0/.pylintrc
https://github.com/OCA/l10n-brazil/blob/12.0/.pylintrc-mandatory

Esses arquivos são configurações de validações executadas pelo commando:

`pre-commit run -a` Como descrito em https://github.com/OCA/maintainer-tools/wiki/Migration-to-version-13.0#technical-method-to-migrate-a-module-from-120-to-130-branch

Para garantir que os commits não serão feitos sem a execução deste comando, você pode inclui-lo no hook do git do seu computador:

```
pip install pre-commit 
git config --global core.hooksPath ~/.git/hooks
mkdir -p ~/.git/hooks
touch ~/.git/hooks/pre-commit
chmod +x ~/.git/hooks/pre-commit
```
Crie um arquivo de pre-commit com a seguinte conteúdo:
```
#!/bin/sh
pre-commit run -a
```
Agora é só commitar que as validações são executadas automaticamente

![Image](https://i.imgur.com/jccdfBO.png)

Bom código!

