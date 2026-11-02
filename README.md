# Registro de Personas (GitHub Pages + Firebase)

Sitio estático: un solo `index.html` que habla directo con Firestore. No depende de Apps Script ni de ningún servidor propio.

## Puesta en marcha

1. **Firebase**: la configuración (`firebaseConfig`) ya está en `index.html`.
2. **Autenticación**: Authentication > *Sign-in method* > habilitar **Google**.
3. **Dominio autorizado**: Authentication > Configuración > *Dominios autorizados* > agregar `fra801.github.io`.
4. **Reglas**: Firestore > Reglas. Pegar el contenido de `firestore.rules` y **Publicar**. Solo los mails de la lista pueden leer y escribir.
5. **GitHub Pages**: Settings > Pages > *Deploy from a branch* > `main` / `(root)`.

## Seguridad

La `apiKey` de Firebase en un sitio web **no es secreta**. Lo que protege los datos son las **reglas de Firestore** y el login con Google.

## Búsqueda

- **Principal**: por DNI (prefijo).
- **Avanzada**: apellido y/o nombre (al menos uno), más edad desde/hasta y localidad (desplegables) para acotar.
- Los datos se buscan en MAYÚSCULAS, tal cual están en la base.

## Listas de localidades y edad máxima

Se guardan en el documento `meta/filtros`. Se generan desde la propia app: en *Búsqueda avanzada*, tocar **Generar ahora** (o **Actualizar listas** cuando se agreguen localidades nuevas). Cuesta aproximadamente una lectura por localidad distinta, no por persona.

## Índices de Firestore

Al buscar con una localidad elegida, Firebase pide un índice compuesto (apellido + localidad, y nombre + localidad). La app muestra un botón **Crear índice**: confirmarlo y esperar 1–2 minutos. Es un paso único.
