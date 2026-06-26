# Sitio Quartz — notas de C++

Este directorio es un sitio [Quartz v5](https://quartz.jzhao.xyz/) que publica el
vault de Obsidian de C++ como web. El contenido (`content/`) **no** vive aquí: es
un **submódulo git** que apunta al repositorio de las notas.

Este repo y el vault son **directorios hermanos e independientes**:

```
Obdisian_General/
├── C++_Obsidian/           ← repo de las NOTAS (el vault de Obsidian)
└── C++_Obsidian_Quartz/    ← ESTE repo (el sitio Quartz)
    ├── content/            ← submódulo git → repo de las notas (C++_Obsidian)
    ├── quartz.config.yaml  ← configuración + tema (colores light/dark de Obsidian)
    └── quartz/styles/custom.scss  ← CSS que replica los snippets del vault
```

El tema reproduce los dos snippets del vault:

- **Modo claro** → `clion-light` (CLion / IntelliJ Light)
- **Modo oscuro** → `cpp-console` (terminal azul-noche, Blue Topaz night)

---

## 1. Conectar el submódulo a tu repositorio (lo único pendiente)

Ahora mismo `content/` ya está poblado con tus notas (clonadas del vault local),
así que el sitio **ya compila y se puede previsualizar**. Pero la URL del submódulo
en `.gitmodules` es un **placeholder**. Cuando crees el repo remoto de las notas:

```bash
# 1) Sube el VAULT (el directorio hermano C++_Obsidian) a su repo remoto:
cd ../C++_Obsidian
git remote add origin <URL-DEL-REPO-DE-NOTAS>
git push -u origin main

# 2) Vuelve a este sitio y pon esa misma URL en el submódulo:
cd ../C++_Obsidian_Quartz
git submodule set-url content <URL-DEL-REPO-DE-NOTAS>
git submodule sync content
git commit -am "content: apuntar el submódulo al repo de notas"
```

A partir de ahí, **clonar el sitio en otra máquina** es:

```bash
git clone --recurse-submodules <URL-DEL-REPO-QUARTZ>
```

> Si ya clonaste sin `--recurse-submodules`:
> `git submodule update --init --recursive`

### Actualizar el contenido tras editar las notas

Cuando añadas o cambies notas en el vault y hagas push de ese repo, actualiza el
puntero del submódulo en el sitio:

```bash
# desde la raíz de este sitio (C++_Obsidian_Quartz)
git -C content pull origin main      # trae las notas nuevas
git add content
git commit -m "content: actualizar notas"
```

---

## 2. Previsualizar / construir en local

```bash
# desde la raíz de este sitio (C++_Obsidian_Quartz)
npm install                            # solo la primera vez
npx quartz plugin install --from-config  # baja los plugins (solo 1ª vez)
npx quartz build --serve               # http://localhost:8080
```

Para una build de producción (genera `public/`):

```bash
npx quartz build
```

---

## 3. Publicar

Antes de publicar, ajusta `baseUrl` en `quartz.config.yaml` a tu dominio real
(p. ej. `tu-usuario.github.io/tu-repo`). Después sigue la guía oficial de hosting:
<https://quartz.jzhao.xyz/hosting>. Para GitHub Pages, recuerda usar
`git clone --recurse-submodules` en la Action de despliegue.
