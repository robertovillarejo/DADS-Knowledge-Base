## Reemplazar el branch principal Master con otro de otro branch

#### Contexto:

Se cuenta con el branch `master` y  el branch`feature`. se desea reemplazar todo el contenido de `feature` en `master`.

#### Script
```bash
git checkout feature
git merge -s ours master
git checkout master
git merge feature
```
