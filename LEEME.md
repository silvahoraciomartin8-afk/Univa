# UNIVA · Guía para publicarla (web + app)

UNIVA es una sola app que funciona de tres maneras:

- **Página web**: se abre desde cualquier navegador con un link (para compartir en Instagram y Facebook).
- **App en el celular**: desde la web se instala con un toque ("Instalar" en Android, "Agregar a inicio" en iPhone). Queda con su ícono y se abre a pantalla completa.
- **App en Google Play** (opcional): con el mismo código se genera el paquete para la tienda.

Los datos (organizaciones, pedidos, colaboraciones) se guardan en **Firebase**, el servicio de Google, que tiene un plan gratis suficiente para empezar.

> Mientras no hagas el Paso 1, la app abre en **modo demostración** con datos de ejemplo. Así podés probarla igual.

---

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La app completa |
| `manifest.json` | Nombre, colores e ícono de la app instalable |
| `sw.js` | Permite instalarla y abrirla sin conexión |
| `icons/` | Íconos de UNIVA en varios tamaños |
| `firestore.rules` | Reglas de seguridad de la base de datos |

---

## Paso 1 · Crear el proyecto en Firebase (15 minutos)

1. Entrá a **console.firebase.google.com** con tu cuenta de Google y tocá **Crear un proyecto**. Nombre: `univa`. Podés desactivar Google Analytics.
2. **Base de datos**: menú *Compilación → Firestore Database → Crear base de datos*. Ubicación: `southamerica-east1 (São Paulo)`. Elegí *Modo de producción*.
3. **Reglas**: en Firestore, pestaña **Reglas**. Borrá lo que hay, pegá todo el contenido de `firestore.rules` y tocá **Publicar**.
4. **Inicio de sesión**: *Compilación → Authentication → Comenzar → Google → Habilitar*. Elegí tu mail como correo de asistencia y guardá.
5. **Registrar la app web**: tocá el engranaje ⚙️ → *Configuración del proyecto* → abajo, en "Tus apps", el ícono **`</>`** (Web). Nombre: `UNIVA`. No marques Hosting.
6. Firebase te muestra un bloque `const firebaseConfig = { apiKey: ..., ... }`. Copiá esos valores.
7. Abrí `index.html` con un editor de texto (el Bloc de notas sirve), buscá `FIREBASE_CONFIG` (está cerca del principio del código) y reemplazá cada `PEGAR_ACA` por tus valores. Guardá.

> Estos datos no son secretos: es normal que estén en la página. La seguridad la dan las reglas del punto 3.

## Paso 2 · Subirla a internet con GitHub Pages (como Voxin)

1. En GitHub, creá un repositorio nuevo, por ejemplo `univa`, público.
2. **Add file → Upload files**: subí `index.html`, `manifest.json`, `sw.js` y la carpeta `icons` (arrastrá todo junto). *Commit changes*.
3. **Settings → Pages**: en *Branch* elegí `main` y carpeta `/ (root)` → **Save**.
4. En uno o dos minutos queda online en `https://TU-USUARIO.github.io/univa/`.
5. **Importante**: volvé a Firebase → *Authentication → Configuración → Dominios autorizados → Agregar dominio* y poné `TU-USUARIO.github.io`. Sin esto no funciona "Continuar con Google".

## Paso 3 · Hacerte administrador

1. Abrí tu UNIVA, tocá **Ingresar** y entrá con Google.
2. Andá a **Mi cuenta**: abajo aparece *Tu código de usuario*. Copialo.
3. En Firebase → Firestore → **Iniciar colección**: ID de colección `admins`. ID del documento: pegá tu código. Agregá un campo cualquiera, por ejemplo `nombre` = `Horacio`. Guardar.
4. Recargá la app: en Mi cuenta vas a ver "Administrador de UNIVA". Como administrador podés editar o borrar cualquier organización y **marcarlas como ✓ Verificadas**.

## Paso 4 · Instalarla en el celular

- **Android (Chrome)**: abrí el link → aparece el cartel "Instalá UNIVA" → **Instalar**. También desde el menú ⋮ → *Instalar app*.
- **iPhone (Safari)**: abrí el link → botón **Compartir** ⬆️ → **Agregar a inicio**.

## Paso 5 (opcional) · Publicarla en Google Play

1. Entrá a **pwabuilder.com**, pegá el link de tu UNIVA y tocá *Start*.
2. *Package for stores → Android → Generate*. Descargás un `.aab` listo para la tienda (y una clave de firma: **guardala bien**, la vas a necesitar para cada actualización).
3. Creá una cuenta en **Google Play Console** (pago único de USD 25) y subí el `.aab`. Google pide política de privacidad, capturas y descripción.

---

## Cómo funciona la seguridad

- **Cualquier persona** puede ver todo y sumarse a colaborar, sin crear cuenta.
- **Las organizaciones** ingresan con Google. Cada una solo puede editar o borrar **sus propios** datos y pedidos.
- **El teléfono o mail** de quien colabora solo lo ve la organización que recibe la ayuda.
- **Solo el administrador** puede marcar una organización como verificada.
- El dinero nunca pasa por UNIVA: se transfiere directo al alias de la organización.

## Actualizar la app

Cuando cambies `index.html`, abrí `sw.js` y subí la versión (`univa-v1` → `univa-v2`). Así los celulares que la tienen instalada reciben la versión nueva.

## Costos

El plan gratis de Firebase (Spark) incluye 50.000 lecturas y 20.000 escrituras por día, suficiente para una ciudad. GitHub Pages es gratis. Un dominio propio (por ejemplo `univa.com.ar` en NIC Argentina) es opcional y se configura en Settings → Pages → Custom domain.
