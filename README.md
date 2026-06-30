# PartyRooms

Plataforma web de minijuegos multijugador (Bingo, UNO, Trivia, Pictionary y más). Este repositorio contiene la **landing page** y la estructura inicial del sitio, lista para publicarse en GitHub Pages.

## 📁 Estructura

```
partyrooms/
├── index.html          # Landing page principal
├── login.html          # Placeholder de inicio de sesión
├── assets/
│   ├── css/style.css   # Estilos (tema oscuro + glassmorphism)
│   ├── js/main.js      # Interacciones de la landing
│   └── img/            # Imágenes (vacío por ahora)
└── README.md
```

## 🚀 Cómo publicarlo en GitHub Pages

1. Crea un repositorio nuevo en GitHub. Si quieres que quede en `https://tu-usuario.github.io/` (sin subcarpeta), el repo debe llamarse exactamente `tu-usuario.github.io`. Si prefieres un nombre como `partyrooms`, el sitio quedará en `https://tu-usuario.github.io/partyrooms/`.
2. Sube **todo el contenido de esta carpeta** (no la carpeta misma, sino lo que está dentro) a la raíz del repositorio.
   - Vía web: arrastra los archivos en la sección "Add file → Upload files" del repo.
   - Vía terminal:
     ```bash
     git init
     git add .
     git commit -m "Primera versión de PartyRooms"
     git branch -M main
     git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
     git push -u origin main
     ```
3. En GitHub, entra a **Settings → Pages**.
4. En "Build and deployment", selecciona **Source: Deploy from a branch**, branch **main**, carpeta **/(root)**.
5. Guarda y espera 1–2 minutos. Tu sitio quedará disponible en la URL que GitHub te muestre.

## 🧱 Qué es esto (y qué no es)

Esto es **solo el frontend estático**: landing page funcional, diseño visual y estructura de navegación. GitHub Pages no puede ejecutar un servidor, así que funciones como login real, salas en tiempo real, base de datos de usuarios o el panel de administración **necesitan un backend** (recomendado: Firebase o Supabase, ambos gratuitos en su plan básico y compatibles con un sitio 100% estático).

## 🔐 Sistema de cuentas (Firebase)

El login, registro y perfil de usuario ya están programados y funcionando en `login.html`, `dashboard.html` y `assets/js/auth.js`. Solo falta conectar tu proyecto de Firebase:

1. Crea un proyecto en [console.firebase.google.com](https://console.firebase.google.com).
2. Activa **Authentication** → métodos "Correo/Contraseña" y "Google".
3. Activa **Firestore Database** (modo producción).
4. Copia tu `firebaseConfig` y pégalo en `assets/js/firebase-config.js`.
5. **Importante — Reglas de seguridad de Firestore:** ve a Firestore → pestaña "Reglas" y reemplaza el contenido por esto, para que cada usuario solo pueda leer/editar su propio perfil:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```

6. En Authentication → Settings → "Authorized domains", agrega tu dominio de GitHub Pages (ej. `tu-usuario.github.io`) para que el login con Google funcione ahí también.

Con eso, `login.html` permite crear cuenta, iniciar sesión y entrar con Google; al primer ingreso se crea automáticamente un documento de perfil en Firestore (nivel, coins, gems, amigos), visible ya en `dashboard.html`.

## 🗺️ Próximos pasos sugeridos

- [x] Conectar Firebase Auth (Google / correo) en `login.html` — falta solo pegar tu `firebaseConfig`.
- [ ] Conectar el formulario de lista de espera (`assets/js/main.js`) a un backend real (opcional, ya que el login cumple esa función).
- [ ] Construir el primer juego jugable (recomendado: Tic Tac Toe o Piedra, Papel o Tijera, por ser los más simples).
- [ ] Crear el sistema de salas con Firestore (sala = documento, jugadores = subcolección).
- [ ] Diseñar el dashboard del usuario una vez exista login real.

## 🎨 Sistema de diseño

- **Colores:** fondo `#0a0c16`, acentos azul `#4d6bff`, morado `#9b5cff`, cian `#34e5ff`.
- **Tipografías:** Outfit (títulos), Inter (texto), JetBrains Mono (códigos de sala, etiquetas).
- **Estilo:** tema oscuro, tarjetas glassmorphism, totalmente responsive.
