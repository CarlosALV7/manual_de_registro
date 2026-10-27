# Portal de proveedores: cómo publicarlo y ligarlo a Power BI

Esta carpeta ya es un sitio web completo. No necesita servidor ni base de datos.

```
portal-proveedores/
├── index.html          ← el portal (HTML + CSS + JS en un solo archivo)
├── img/                ← capturas de pantalla de cada paso (sacadas de los PDF)
└── manuales/           ← los PDF que se descargan desde el portal
    ├── manual-mexico.pdf
    ├── manual-latinoamerica.pdf
    └── manual-usa-europe.pdf
```

## 1. Personalizar

Abra `index.html` y busque el bloque `CONFIG` (dentro de `<script>`):

- `company`: nombre de la empresa que aparece en todo el portal.
- `supportEmail` / `supportPhone` / `supportHours`: datos del Centro de ayuda.
- `bookingUrl`: liga para agendar (Microsoft Bookings, Teams o Calendly). Solo se muestra a proveedores de México ("Llamada directa con nuestro equipo").
- `pdfs`: qué PDF se descarga en cada región.

El portal no pide ningún dato al proveedor: solo muestra información y los medios de contacto.

Los pasos con capturas están en `FLOWS`; la guía del cuestionario pregunta por pregunta está en `QGUIDE`; las claves bancarias de México (3 primeros dígitos de la CLABE, banco y SWIFT) están en `BANKS_MX` (para cambiar una imagen, reemplace el archivo en `img/` con el mismo nombre). Las preguntas frecuentes están en `FAQ`. Lo demás está en `BANK` y `DOCS`, siempre en pares `es` / `en`.
Para agregar un país, añada una línea en `COUNTRIES` y asígnele una región (`mx`, `latam`, `sv` o `na`).

## 2. Publicarlo para acceso mundial (gratis)

| Opción | Cómo | Ideal si… |
|---|---|---|
| **Netlify Drop** | Entre a app.netlify.com/drop y arrastre la carpeta `portal-proveedores`. Le da una URL `https://….netlify.app` en segundos. | Quiere algo rápido sin instalar nada. |
| **GitHub Pages** | Suba la carpeta a un repositorio y active *Settings → Pages*. URL `https://usuario.github.io/repo`. | Quiere historial de cambios y que otros editen. |
| **Azure Static Web Apps / SharePoint** | Opción corporativa si TI prefiere mantenerlo en Microsoft 365 / Azure. | La empresa exige hospedaje interno. |

Todas usan CDN global y HTTPS, así que el proveedor lo abre igual desde México, Colombia o Europa.

## 3. Enlaces directos por país e idioma

El portal lee parámetros en la URL:

```
https://SU-SITIO/?pais=mx&lang=es
https://SU-SITIO/?pais=co&lang=es
https://SU-SITIO/?pais=us&lang=en
```

Códigos de país: `mx`, `co`, `cr`, `sv`, `gt`, `hn`, `pa`, `pe`, `us`, `eu`.

## 4. Ligarlo a Power BI

- **En un reporte:** agregue un botón o imagen → *Acción* → *Tipo: URL web* → pegue la URL del portal.
- **Enlace dinámico por proveedor:** cree una medida con la URL, por ejemplo
  `URL Manual = "https://SU-SITIO/?pais=" & SELECTEDVALUE(Proveedores[CodigoPais]) & "&lang=es"`,
  y úsela en la acción del botón (*fx*) o como columna con *Categoría de datos → URL web* en una tabla.
- **En un panel (dashboard) del servicio de Power BI:** *Editar → Agregar icono → Contenido web* y pegue
  `<iframe src="https://SU-SITIO/?pais=mx&lang=es" width="100%" height="600"></iframe>`.

Nota: los reportes de Power BI no muestran páginas externas dentro del lienzo de forma nativa; lo más estable es el botón con URL o el icono de contenido web del panel.
