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
- **Avanzada**: apellido, nombre y/o calle (al menos uno), más edad desde/hasta y localidad (desplegables) para acotar.
- **Calle**: se busca por prefijo, así que hay que escribirla como empieza en la base (ej. `BELGRANO`). Si se combina con apellido o nombre, en cambio, busca el texto en cualquier parte del nombre de la calle.
- Los datos se buscan en MAYÚSCULAS, tal cual están en la base.

## Datos y colección

Los datos están en la colección `padron` y el ID de cada documento es el **número de documento** (si un número está repetido entre personas distintas, el ID es `número_tipo_sexo`). Se va cargando con `migrar.py` (19.000 registros por día en el plan gratis). La app trabaja únicamente con lo que hay en `padron`; la colección vieja `personas` ya no se usa.

Campos: apellido, nombre, sexo, apodo, tipoDoc, numDoc, fnac, ffall, edad, barrio, modulo, escalera, calle, domNum, piso, domDepto, localidad, depto, provincia, apellidoPadre, nombresPadre, padreVive, apellidoMadre, nombresMadre, madreVive, apellidoHijo1, nombresHijo1, apellidoHijo2, nombresHijo2, profesion, telefono, obs.

## Ficha

En la ficha **solo se muestran los campos que tienen datos** (y las secciones que tengan al menos uno). Al tocar *Editar* aparecen todos los campos para poder completarlos. Al crear un registro nuevo, el ID es el número de documento.

## Listas desplegables

El formulario (alta y edición) ofrece desplegables de **tipo de documento, localidad, departamento y provincia**, con los valores que ya existen en la base. Las búsquedas usan la misma lista de localidades y la edad máxima.

Se guardan en el documento `meta/filtros` (`edadMax`, `localidades`, `departamentos`, `provincias`, `tiposDoc`) y se generan desde la propia app: la primera vez que faltan se arman solas, y después, en *Búsqueda avanzada*, tocar **Actualizar listas** cuando aparezcan valores nuevos en la base. Cuesta aproximadamente una lectura por valor distinto, no por persona.

Si una persona ya tiene un valor que no figura en la lista, la ficha lo conserva al editar. En un registro nuevo hay que elegir el tipo de documento.

## Índices de Firestore

Al buscar con una localidad elegida, Firebase pide un índice compuesto (apellido + localidad, nombre + localidad y calle + localidad). La app muestra un botón **Crear índice**: confirmarlo y esperar 1–2 minutos. Es un paso único.
