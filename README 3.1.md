# SADA Analytics — Sistema Integral de Análisis Académico

Prototipo SPA (Single Page Application) en **HTML + CSS + JavaScript puro**, un solo archivo (`SADA_Professional_v31.html`), sin backend ni servidor. Toda la información se guarda en el `localStorage` del navegador. Pensado para sustentación académica del proyecto SADA S.A.C. (arquitectura empresarial / EdTech).

---

## 🔑 Credenciales de acceso (usuarios de prueba precargados)

> Estas cuentas se crean automáticamente la primera vez que se abre el archivo (función `initDB()` → `DEF_USERS`). Si el `localStorage` ya tiene datos previos, estas cuentas **no se vuelven a crear**; usa el botón "Restablecer datos de prueba" en Configuración (solo Admin) si necesitas regenerarlas.

| Rol | Nombre | Correo | Contraseña | Notas |
|---|---|---|---|---|
| **Administrador** | Administrador SADA | `admin@sada.edu.pe` | `Admin123` | Acceso total al sistema |
| **Docente** | Juan García López | `juan.garcia@escuela.edu.pe` | `Pass123` | Dicta 6 cursos en I.E. Innova Schools (Matemática, Comunicación, CTA, Computación, Inglés, Ed. Física) |
| **Docente** | Pedro Rojas | `pedro.rojas@escuela.edu.pe` | `Pass123` | Dicta 2 cursos en I.E. José Olaya (Ciencias Sociales, Arte) |
| **Padre de familia** | María López Pérez | `maria.lopez@familia.com.pe` | `Pass123` | Vinculada al estudiante Carlos Quispe (U004) |
| **Estudiante** | Carlos Quispe | `carlos@escuela.edu.pe` | `Pass123` | Hijo de María López. 8 notas registradas |
| **Estudiante** | Ana Torres | `ana.torres@escuela.edu.pe` | `Pass123` | I.E. José Olaya. 4 notas registradas |

**Registro público:** solo permite crear cuentas de rol **Estudiante** (por seguridad). Docentes, padres y administradores únicamente los crea el Admin desde el módulo Usuarios.

**Usuarios creados manualmente desde el panel Admin** reciben la contraseña temporal: `Temp123` (se muestra en el propio formulario de creación).

---

## 🌐 Cómo usarlo

1. Abrir `SADA_Professional_v31.html` directamente en el navegador (doble clic) o subirlo a cualquier hosting estático.
2. No requiere instalación, npm, ni conexión a internet — excepto la librería SheetJS (para exportar Excel), que se carga desde CDN.
3. Iniciar sesión con cualquiera de las credenciales de la tabla anterior para explorar el sistema según cada rol.
4. Los datos persisten en el navegador (localStorage). Para reiniciar todo el prototipo desde cero, borrar los datos del sitio en el navegador o limpiar el `localStorage` de la pestaña.

---

## 👥 Roles y matriz de permisos por módulo

| Módulo | Admin | Docente | Estudiante | Padre |
|---|:---:|:---:|:---:|:---:|
| Dashboard | ✅ | ✅ | ✅ | ✅ |
| Instituciones | ✅ | ❌ | ❌ | ❌ |
| Usuarios / Alumnos | ✅ (CRUD total) | ✅ (solo lectura de sus alumnos) | ❌ | ❌ |
| Cursos | ✅ (CRUD) | ✅ (lectura de los propios) | ✅ (lectura, los suyos) | ✅ (lectura, los del hijo) |
| Notas (calificaciones) | ✅ (CRUD total) | ✅ (crear/editar/eliminar **solo en sus cursos y alumnos**) | ✅ (solo lectura, las suyas) | ✅ (solo lectura, las del hijo) |
| **Observaciones** *(nuevo)* | ✅ (ver/eliminar todas) | ✅ (crear/editar/eliminar **solo las que él mismo escribió**, sobre sus alumnos) | ✅ (solo lectura, las suyas) | ✅ (solo lectura, las del hijo) |
| Análisis de desempeño | ✅ (generar + ver todo) | ✅ (ver de sus alumnos + PDF) | ✅ (ver el suyo + PDF) | ✅ (ver el del hijo + PDF) |
| Reportes oficiales / Exportar Excel | ✅ | ❌ | ❌ | ❌ |
| Orientación vocacional | ✅ | ✅ | ✅ | ✅ |
| Integraciones / MVP | ✅ | ❌ | ❌ | ❌ |
| Configuración | ✅ | ❌ | ❌ | ❌ |

La visibilidad del menú y de cada página se valida en dos capas: (1) el menú lateral solo muestra los módulos permitidos (`PAGE_ACCESS`), y (2) cada acción de escritura (crear, editar, eliminar) se revalida en el propio código antes de ejecutarse, para que un intento de acceso directo por URL/consola no permita saltarse los permisos.

---

## 🧩 Módulos del sistema

- **Dashboard** — KPIs según alcance del rol (instituciones, usuarios/alumnos visibles, cursos, notas, análisis, reportes) y resumen de módulos disponibles.
- **Instituciones** (solo Admin) — Registro de colegios.
- **Usuarios / Alumnos** — Gestión de cuentas (Admin) o consulta de alumnos relacionados a los cursos (Docente). Incluye botón rápido **🗒️ Observación** por alumno.
- **Cursos** — Asignaturas, docente asignado y periodo académico.
- **Notas** — Registro de calificaciones (0–20) por estudiante, curso y tipo de evaluación (Parcial, Final, Práctica, Tarea). Docente y Admin pueden **crear, editar (✏️) y eliminar (×)** dentro de su alcance.
- **Observaciones** *(módulo nuevo)* — Comentarios de seguimiento del docente hacia cada alumno, con tipo (Positiva / Área de mejora / Conducta / Asistencia). Visible para el propio alumno y su padre/tutor como bitácora de acompañamiento.
- **Análisis de desempeño** — Cálculo automático de promedio, tendencia, riesgo de deserción y plan de mejora semanal. Descarga en PDF offline (sin dependencias externas).
- **Reportes institucionales** (solo Admin) — Generación oficial y exportación a Excel (SheetJS).
- **Orientación vocacional** — Recomendación de área de afinidad (Ingeniería/Tecnología, Humanidades, Artes, etc.) según notas por curso, con frase motivacional y PDF descargable.
- **Integraciones / MVP** (solo Admin) — Pantalla demostrativa de cómo SADA recibiría datos desde plataformas LMS externas (Moodle/Canvas vía LTI/xAPI).
- **Configuración** (solo Admin) — Gestión de usuarios y ajustes generales.

### 📥 Descarga de informes en PDF
Al hacer clic en "Descargar PDF" (en Análisis o Reportes), si el usuario tiene **más de un alumno visible** (Admin, Docente o Padre con varios hijos), el sistema pregunta el alcance de la descarga:
- **Todos los alumnos visibles**, o
- **Un alumno específico** (selector).

Si solo hay un alumno visible (caso típico de Estudiante o Padre con un solo hijo), el PDF se descarga directo sin preguntar.

---

## 🗄️ Modelo de datos (7 entidades + extensión)

Basado en el diagrama ER del documento de arquitectura (TOGAF/COBIT/ITIL):

| Entidad | Descripción |
|---|---|
| `INSTITUCION` | Colegios registrados |
| `USUARIO` | Admin, Docente, Estudiante, Padre |
| `CURSO` | Asignaturas y docente asignado |
| `NOTA` | Calificaciones por estudiante/curso |
| `ANALISIS_DESEMPENO` | Promedio, tendencia, riesgo |
| `REPORTE` | Registro de reportes generados |
| `RECOMENDACION_VOCACIONAL` | Resultados de orientación |
| `OBSERVACION` *(extensión v31)* | Comentarios de seguimiento docente→alumno, independiente de las notas académicas, para no alterar el modelo ER original |

### Claves de almacenamiento (localStorage)
```
SADA_USERS            → usuarios
SADA_INSTITUTIONS     → instituciones
SADA_CURSOS           → cursos
SADA_NOTAS            → calificaciones
SADA_MATRICULAS       → matrículas curso-alumno
SADA_ANALYSIS         → análisis de desempeño generados
SADA_REPORTES         → reportes oficiales
SADA_RECOMENDACIONES  → orientación vocacional
SADA_OBSERVACIONES    → observaciones docente-alumno
SADA_LOGGED_USER      → sesión activa
```

---

## ⚙️ Stack técnico

- HTML5 + CSS3 (sin frameworks, un solo archivo)
- JavaScript vanilla (ES6+)
- Persistencia: `localStorage` del navegador (no hay servidor ni base de datos real)
- Exportación Excel: [SheetJS](https://cdnjs.cloudflare.com/ajax/libs/xlsx/) vía CDN
- Generación de PDF: motor propio offline (construcción manual del binario PDF, sin librerías externas como jsPDF)
- Responsive: funciona en escritorio y móvil (menú adaptable, tablas con scroll horizontal)
- Modo claro / oscuro incluido

---

## ⚠️ Notas importantes para la sustentación

- Este es un **prototipo funcional (MVP)**, no un sistema en producción. No usar contraseñas reales ni datos personales reales.
- En producción se requeriría: backend real, hash de contraseñas (no texto plano), base de datos, cifrado en tránsito/reposo y cumplimiento de protección de datos de menores.
- El módulo "Integraciones/MVP" es únicamente demostrativo para explicar cómo se conectaría SADA a un LMS externo (Moodle/Canvas).
- Los datos de ejemplo (usuarios, cursos, notas) se regeneran solo si el `localStorage` está vacío; si ya probaste el sistema antes, tus cambios previos persisten.

---

## 🆕 Cambios recientes (v31)

1. **Corrección crítica de visibilidad de botones**: el botón "+ Registrar nota" (y otros condicionados por rol) no aparecía para ningún usuario debido a un error en la lógica de `display` vía CSS/JS. Corregido.
2. **Edición de notas habilitada**: antes solo se podían crear notas; ahora también se pueden **editar (✏️)** y **eliminar (×)**, respetando que el Docente solo pueda hacerlo sobre sus propios cursos/alumnos.
3. **Nuevo módulo Observaciones**: el docente ahora puede registrar comentarios de seguimiento (no académicos) sobre cada alumno, con edición y eliminación restringida al autor (o Admin).
4. **Descarga de PDF con selección de alcance**: al descargar el informe en PDF, el sistema pregunta si se desea el de todos los alumnos visibles o solo uno en específico (cuando aplica según el rol).

---

*Documento generado como parte de la entrega del prototipo SADA S.A.C. — Arquitectura Empresarial, UTP 2026-I.*
