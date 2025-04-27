# 📚 Post: "Guía Completa de Introducción a Node.js para Principiantes"

---

## Tabla de Contenidos
- [📚 Post: "Guía Completa de Introducción a Node.js para Principiantes"](#-post-guía-completa-de-introducción-a-nodejs-para-principiantes)
  - [Tabla de Contenidos](#tabla-de-contenidos)
- [Introducción](#introducción)
- [¿Qué es Node.js?](#qué-es-nodejs)
- [¿Por qué aprender Node.js?](#por-qué-aprender-nodejs)
- [Instalación de Node.js](#instalación-de-nodejs)
  - [Descargar Node.js](#descargar-nodejs)
  - [Instalar Node.js en Windows](#instalar-nodejs-en-windows)
  - [Instalar Node.js en macOS](#instalar-nodejs-en-macos)
  - [Instalar Node.js en Linux](#instalar-nodejs-en-linux)
    - [Ubuntu / Debian:](#ubuntu--debian)
    - [Fedora:](#fedora)
    - [Arch Linux:](#arch-linux)
- [Verificar la instalación](#verificar-la-instalación)
- [Entender `npm`](#entender-npm)
- [Crear tu primer proyecto con Node.js](#crear-tu-primer-proyecto-con-nodejs)
  - [Iniciar un proyecto con `npm init`](#iniciar-un-proyecto-con-npm-init)
  - [Crear un archivo JavaScript](#crear-un-archivo-javascript)
  - [Ejecutar tu primer programa](#ejecutar-tu-primer-programa)
- [Conceptos Fundamentales de Node.js](#conceptos-fundamentales-de-nodejs)
  - [Módulos](#módulos)
  - [Sistema de archivos (`fs`)](#sistema-de-archivos-fs)
  - [Eventos](#eventos)
  - [Entorno no bloqueante (non-blocking)](#entorno-no-bloqueante-non-blocking)
- [🛠️ Beginner Projects (1-10) - Node.js](#️-beginner-projects-1-10---nodejs)
  - [1. List all files in a directory](#1-list-all-files-in-a-directory)
  - [2. Create a folder if it doesn't exist](#2-create-a-folder-if-it-doesnt-exist)
  - [3. Create a basic project folder structure](#3-create-a-basic-project-folder-structure)
  - [4. Delete files with a specific extension](#4-delete-files-with-a-specific-extension)
  - [5. Rename multiple files](#5-rename-multiple-files)
  - [6. Copy files to another folder (backup)](#6-copy-files-to-another-folder-backup)
  - [7. Organize downloads by file type](#7-organize-downloads-by-file-type)
  - [8. Check if a file exists before working with it](#8-check-if-a-file-exists-before-working-with-it)
  - [9. Monitor directory changes](#9-monitor-directory-changes)
  - [10. Remove empty folders](#10-remove-empty-folders)
- [🛠️ Intermediate Projects (11-20) - Node.js](#️-intermediate-projects-11-20---nodejs)
  - [11. Find and replace text in multiple files](#11-find-and-replace-text-in-multiple-files)
  - [12. Move specific files to a backup folder](#12-move-specific-files-to-a-backup-folder)
  - [13. Synchronize two folders](#13-synchronize-two-folders)
  - [14. Create a daily backup folder with timestamp](#14-create-a-daily-backup-folder-with-timestamp)
  - [15. List large files in a directory](#15-list-large-files-in-a-directory)
  - [16. Check file permissions](#16-check-file-permissions)
  - [17. Generate a report of all file sizes](#17-generate-a-report-of-all-file-sizes)
  - [18. Identify duplicate filenames](#18-identify-duplicate-filenames)
  - [19. Change file extensions in bulk](#19-change-file-extensions-in-bulk)
  - [20. Simulate a "Trash Bin" for deleted files](#20-simulate-a-trash-bin-for-deleted-files)
- [🛠️ Advanced Projects (21-30) - Node.js](#️-advanced-projects-21-30---nodejs)
  - [21. Zip an entire folder](#21-zip-an-entire-folder)
  - [22. Extract all files from a zip archive](#22-extract-all-files-from-a-zip-archive)
  - [23. Set environment variables programmatically](#23-set-environment-variables-programmatically)
  - [24. Create an installer script for a Node.js project](#24-create-an-installer-script-for-a-nodejs-project)
  - [25. Create a dynamic sitemap of all HTML files](#25-create-a-dynamic-sitemap-of-all-html-files)
  - [26. Create a basic cronjob-like scheduler](#26-create-a-basic-cronjob-like-scheduler)
  - [27. Create a "Safe Delete" script with user confirmation](#27-create-a-safe-delete-script-with-user-confirmation)
  - [28. Create a file integrity checker (SHA256)](#28-create-a-file-integrity-checker-sha256)
  - [29. Mirror directory structure without copying files](#29-mirror-directory-structure-without-copying-files)
  - [30. Detect and remove broken symbolic links](#30-detect-and-remove-broken-symbolic-links)

---

# Introducción

Node.js es una plataforma que te permite ejecutar JavaScript fuera del navegador. Es ampliamente utilizado para crear servidores web, aplicaciones en tiempo real y herramientas de automatización.

Esta guía cubre todo lo que necesitas para **comenzar de forma profesional con Node.js**, incluso si es tu primera vez.

---

# ¿Qué es Node.js?

**Node.js** es un entorno de ejecución de JavaScript basado en el motor **V8** de Chrome.

Características principales:
- **Asincronía:** Node maneja operaciones de entrada/salida de manera no bloqueante.
- **Ligero y eficiente:** Perfecto para aplicaciones en tiempo real.
- **Enfocado en eventos:** Usa un modelo de eventos para gestionar la concurrencia.

✅ **Ejemplos de uso en la industria:**  
- Servidores web (Express.js)  
- APIs REST  
- Aplicaciones de mensajería (WhatsApp Web usa Node)  
- Automatización de tareas

---

# ¿Por qué aprender Node.js?

- **Demanda laboral:** Node.js está entre los entornos más solicitados para desarrolladores full-stack.
- **Velocidad:** Permite crear aplicaciones muy rápidas.
- **Un solo lenguaje:** Usa JavaScript tanto en el cliente como en el servidor.
- **Ecosistema enorme:** Más de un millón de paquetes disponibles en npm.

---

# Instalación de Node.js

## Descargar Node.js

Visita la página oficial:

👉 [https://nodejs.org/](https://nodejs.org/)

Verás dos opciones:
- **LTS (Long Term Support):** Versión recomendada para la mayoría de usuarios (más estable).
- **Current:** Última versión con las últimas características.

🔵 **Recomendación para principiantes:** **Descargar la versión LTS.**

---

## Instalar Node.js en Windows

1. Descarga el instalador `.msi` de la página oficial.
2. Ejecuta el instalador y sigue los pasos del asistente.
   - Asegúrate de marcar la opción **"Add to PATH"**.
3. Finaliza la instalación.

✅ ¡Listo! Node.js y npm estarán instalados en tu sistema.

---

## Instalar Node.js en macOS

Usa **Homebrew** (recomendado):

```bash
brew install node
```

Si no tienes Homebrew, puedes instalarlo desde: [https://brew.sh/](https://brew.sh/)

Alternativamente, puedes descargar el instalador `.pkg` desde la web oficial y seguir el asistente.

---

## Instalar Node.js en Linux

### Ubuntu / Debian:

```bash
sudo apt update
sudo apt install nodejs npm
```

### Fedora:

```bash
sudo dnf install nodejs
```

### Arch Linux:

```bash
sudo pacman -S nodejs npm
```

🛠️ **Tip:** En sistemas Linux es mejor instalar Node.js usando **nvm** (Node Version Manager), que permite cambiar entre diferentes versiones.

Instalar `nvm`:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```
Luego:
```bash
nvm install --lts
```

---

# Verificar la instalación

Después de instalar, abre la terminal (o CMD) y ejecuta:

```bash
node -v
```
Deberías ver algo como:

```bash
v20.5.1
```

Para verificar `npm`:

```bash
npm -v
```
Algo como:

```bash
9.7.2
```

✅ Si ves versiones, ¡Node.js está correctamente instalado!

---

# Entender `npm`

**npm** significa **Node Package Manager**. Es el gestor de paquetes oficial de Node.js.

Permite:
- Instalar librerías.
- Gestionar dependencias de tu proyecto.
- Crear scripts para automatizar tareas.

Ejemplo de instalar una librería:

```bash
npm install express
```

Esto descargará `express` y lo añadirá a tu proyecto.

---

# Crear tu primer proyecto con Node.js

## Iniciar un proyecto con `npm init`

1. Crea una carpeta para tu proyecto:

```bash
mkdir my-first-node-project
cd my-first-node-project
```

2. Inicializa el proyecto:

```bash
npm init
```

(npm te hará algunas preguntas; puedes aceptar valores por defecto.)

🛠️ También puedes hacerlo más rápido:

```bash
npm init -y
```
(Este comando autocompleta todo.)

Esto creará un archivo `package.json`.

---

## Crear un archivo JavaScript

Crea un archivo llamado `index.js`:

```javascript
console.log("Hello World from Node.js!");
```

---

## Ejecutar tu primer programa

Desde la terminal:

```bash
node index.js
```

Verás:

```
Hello World from Node.js!
```

🎉 ¡Acabas de ejecutar tu primer programa en Node.js!

---

# Conceptos Fundamentales de Node.js

## Módulos

Node usa módulos para organizar el código.

Ejemplo de importar el módulo `fs`:

```javascript
const fs = require('fs');
```

✅ Puedes importar librerías propias, de terceros, o del sistema.

---

## Sistema de archivos (`fs`)

`fs` permite trabajar con archivos: leerlos, escribirlos, eliminarlos, etc.

Ejemplo de leer un archivo:

```javascript
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

---

## Eventos

Node.js se basa en eventos.  
Puedes crear tus propios eventos usando el módulo `events`.

```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.on('message', () => {
  console.log('Message event triggered!');
});

emitter.emit('message');
```

---

## Entorno no bloqueante (non-blocking)

Node usa operaciones **asíncronas** para no bloquear el programa.

Por ejemplo, al leer un archivo, Node no espera que termine para seguir ejecutando otras líneas.

✅ Esto es crucial para crear servidores rápidos y eficientes.

---

# 🛠️ Beginner Projects (1-10) - Node.js

---

## 1. List all files in a directory

**Objective:**  
Use `fs.readdir` to list all files and directories.

```javascript
const fs = require('fs');
const path = './';

fs.readdir(path, (err, files) => {
  if (err) {
    console.error('Error reading the directory', err);
    return;
  }
  files.forEach(file => console.log(file));
});
```

✅ **Applications:** Directory inspection, media/resource loading.

---

## 2. Create a folder if it doesn't exist

**Objective:**  
Avoid errors by checking the existence of a folder before creating it.

```javascript
const fs = require('fs');
const folderName = './new_folder';

if (!fs.existsSync(folderName)) {
  fs.mkdirSync(folderName);
  console.log('Folder created:', folderName);
} else {
  console.log('Folder already exists:', folderName);
}
```

✅ **Applications:** Project initialization scripts.

---

## 3. Create a basic project folder structure

**Objective:**  
Automate the setup of a typical project structure.

```javascript
const fs = require('fs');

const folders = ['src', 'public', 'public/images', 'public/css', 'routes'];

folders.forEach(folder => {
  if (!fs.existsSync(folder)) {
    fs.mkdirSync(folder, { recursive: true });
    console.log('Created folder:', folder);
  }
});
```

✅ **Applications:** Web development, boilerplate generation.

---

## 4. Delete files with a specific extension

**Objective:**  
Automatically delete unwanted files such as `.tmp`.

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    if (file.endsWith('.tmp')) {
      fs.unlinkSync(path.join(directory, file));
      console.log('Deleted file:', file);
    }
  });
});
```

✅ **Applications:** Cleaning build artifacts, cache files.

---

## 5. Rename multiple files

**Objective:**  
Batch rename files by adding a prefix.

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';
const prefix = 'renamed_';

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    if (file.endsWith('.txt')) {
      const oldPath = path.join(directory, file);
      const newPath = path.join(directory, prefix + file);
      fs.renameSync(oldPath, newPath);
      console.log(`Renamed ${file} to ${prefix + file}`);
    }
  });
});
```

✅ **Applications:** Dataset cleaning, file standardization.

---

## 6. Copy files to another folder (backup)

**Objective:**  
Backup critical files automatically.

```javascript
const fs = require('fs');
const path = require('path');

const source = './';
const backup = './backup/';

if (!fs.existsSync(backup)) {
  fs.mkdirSync(backup);
}

fs.readdir(source, (err, files) => {
  files.forEach(file => {
    if (file.endsWith('.js')) {
      fs.copyFileSync(path.join(source, file), path.join(backup, file));
      console.log('Copied:', file);
    }
  });
});
```

✅ **Applications:** Project versioning, backups before updates.

---

## 7. Organize downloads by file type

**Objective:**  
Sort files into categorized folders based on extensions.

```javascript
const fs = require('fs');
const path = require('path');

const downloads = './downloads/';
const folders = {
  pdf: 'pdfs',
  jpg: 'images',
  png: 'images',
  zip: 'archives'
};

fs.readdir(downloads, (err, files) => {
  files.forEach(file => {
    const ext = path.extname(file).slice(1);
    if (folders[ext]) {
      const destFolder = path.join(downloads, folders[ext]);
      if (!fs.existsSync(destFolder)) {
        fs.mkdirSync(destFolder, { recursive: true });
      }
      fs.renameSync(path.join(downloads, file), path.join(destFolder, file));
      console.log('Moved', file, 'to', destFolder);
    }
  });
});
```

✅ **Applications:** Digital organization, office automation.

---

## 8. Check if a file exists before working with it

**Objective:**  
Prevent errors by ensuring the file is available.

```javascript
const fs = require('fs');

const filePath = './config.json';

if (fs.existsSync(filePath)) {
  console.log('File exists:', filePath);
} else {
  console.log('File does not exist:', filePath);
}
```

✅ **Applications:** Safer file operations, dynamic config loading.

---

## 9. Monitor directory changes

**Objective:**  
Detect changes such as new files or deleted files in a folder.

```javascript
const fs = require('fs');

const folder = './';

fs.watch(folder, (eventType, filename) => {
  if (filename) {
    console.log(`${eventType} detected on ${filename}`);
  }
});
```

✅ **Applications:** File watchers, automated update triggers.

---

## 10. Remove empty folders

**Objective:**  
Clean project structures by deleting empty folders.

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';

function removeEmptyDirs(dir) {
  const files = fs.readdirSync(dir);
  if (files.length === 0) {
    fs.rmdirSync(dir);
    console.log('Deleted empty folder:', dir);
    return;
  }
  files.forEach(file => {
    const fullPath = path.join(dir, file);
    if (fs.statSync(fullPath).isDirectory()) {
      removeEmptyDirs(fullPath);
    }
  });
}

removeEmptyDirs(directory);
```

✅ **Applications:** Project hygiene, repository cleanup.

---

# 🛠️ Intermediate Projects (11-20) - Node.js

---

## 11. Find and replace text in multiple files

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';
const search = 'oldText';
const replace = 'newText';

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    if (file.endsWith('.txt')) {
      const filePath = path.join(directory, file);
      let content = fs.readFileSync(filePath, 'utf8');
      content = content.replace(new RegExp(search, 'g'), replace);
      fs.writeFileSync(filePath, content);
      console.log(`Updated ${file}`);
    }
  });
});
```

---

## 12. Move specific files to a backup folder

```javascript
const fs = require('fs');
const path = require('path');

const source = './';
const backup = './backup/';

if (!fs.existsSync(backup)) {
  fs.mkdirSync(backup);
}

fs.readdir(source, (err, files) => {
  files.forEach(file => {
    if (file.endsWith('.log')) {
      fs.renameSync(path.join(source, file), path.join(backup, file));
      console.log('Moved:', file);
    }
  });
});
```

---

## 13. Synchronize two folders

```javascript
const fs = require('fs');
const path = require('path');

const src = './source';
const dest = './destination';

if (!fs.existsSync(dest)) {
  fs.mkdirSync(dest);
}

fs.readdir(src, (err, files) => {
  files.forEach(file => {
    const srcFile = path.join(src, file);
    const destFile = path.join(dest, file);
    if (!fs.existsSync(destFile)) {
      fs.copyFileSync(srcFile, destFile);
      console.log('Copied:', file);
    }
  });
});
```

---

## 14. Create a daily backup folder with timestamp

```javascript
const fs = require('fs');
const path = require('path');

const today = new Date().toISOString().split('T')[0];
const backupFolder = `./backup_${today}`;

if (!fs.existsSync(backupFolder)) {
  fs.mkdirSync(backupFolder);
}

fs.copyFileSync('important.txt', path.join(backupFolder, 'important.txt'));
console.log('Backup created at:', backupFolder);
```

---

## 15. List large files in a directory

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';
const sizeLimit = 1024 * 1024; // 1MB

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    const filePath = path.join(directory, file);
    if (fs.statSync(filePath).size > sizeLimit) {
      console.log(`${file} is larger than 1MB`);
    }
  });
});
```

---

## 16. Check file permissions

```javascript
const fs = require('fs');

const filePath = './script.js';

console.log('Readable:', fs.existsSync(filePath) && fs.accessSync(filePath, fs.constants.R_OK) === undefined);
console.log('Writable:', fs.existsSync(filePath) && fs.accessSync(filePath, fs.constants.W_OK) === undefined);
console.log('Executable:', fs.existsSync(filePath) && fs.accessSync(filePath, fs.constants.X_OK) === undefined);
```

---

## 17. Generate a report of all file sizes

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';
const report = 'report.txt';

let output = '';

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    const filePath = path.join(directory, file);
    if (fs.statSync(filePath).isFile()) {
      const size = fs.statSync(filePath).size;
      output += `${file}: ${size} bytes\n`;
    }
  });

  fs.writeFileSync(report, output);
  console.log('Report generated:', report);
});
```

---

## 18. Identify duplicate filenames

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';
const seen = new Set();

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    if (seen.has(file)) {
      console.log('Duplicate found:', file);
    } else {
      seen.add(file);
    }
  });
});
```

---

## 19. Change file extensions in bulk

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    if (file.endsWith('.txt')) {
      const baseName = path.basename(file, '.txt');
      const newName = baseName + '.md';
      fs.renameSync(path.join(directory, file), path.join(directory, newName));
      console.log(`Renamed ${file} to ${newName}`);
    }
  });
});
```

---

## 20. Simulate a \"Trash Bin\" for deleted files

```javascript
const fs = require('fs');
const path = require('path');

const trash = './trash';
const fileToDelete = './old_file.txt';

if (!fs.existsSync(trash)) {
  fs.mkdirSync(trash);
}

if (fs.existsSync(fileToDelete)) {
  const filename = path.basename(fileToDelete);
  fs.renameSync(fileToDelete, path.join(trash, filename));
  console.log('Moved to trash:', filename);
} else {
  console.log('File not found:', fileToDelete);
}
```
---

# 🛠️ Advanced Projects (21-30) - Node.js

---

## 21. Zip an entire folder

```javascript
const fs = require('fs');
const path = require('path');
const { exec } = require('child_process');

const folderToZip = './project';
const outputZip = './project_backup.zip';

exec(`zip -r ${outputZip} ${folderToZip}`, (err, stdout, stderr) => {
  if (err) {
    console.error('Error creating zip:', err);
    return;
  }
  console.log('Zip archive created:', outputZip);
});
```

---

## 22. Extract all files from a zip archive

```javascript
const { exec } = require('child_process');

const zipFile = './project_backup.zip';
const outputFolder = './unzipped_project';

exec(`unzip ${zipFile} -d ${outputFolder}`, (err, stdout, stderr) => {
  if (err) {
    console.error('Error extracting zip:', err);
    return;
  }
  console.log('Extracted to:', outputFolder);
});
```

---

## 23. Set environment variables programmatically

```javascript
process.env.MY_SECRET = 'super_secret_value';

console.log('Environment variable MY_SECRET:', process.env.MY_SECRET);
```

---

## 24. Create an installer script for a Node.js project

```javascript
const fs = require('fs');
const path = require('path');

const folders = ['logs', 'src', 'tests', 'public'];

folders.forEach(folder => {
  if (!fs.existsSync(folder)) {
    fs.mkdirSync(folder);
    console.log('Created folder:', folder);
  }
});

fs.writeFileSync('./src/index.js', '// Entry point');
console.log('Project structure initialized.');
```

---

## 25. Create a dynamic sitemap of all HTML files

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';
let sitemap = '';

function generateSitemap(dir) {
  fs.readdirSync(dir).forEach(file => {
    const fullPath = path.join(dir, file);
    if (fs.statSync(fullPath).isDirectory()) {
      generateSitemap(fullPath);
    } else if (file.endsWith('.html')) {
      sitemap += fullPath.replace(/\\/g, '/') + '\n';
    }
  });
}

generateSitemap(directory);

fs.writeFileSync('sitemap.txt', sitemap);
console.log('Sitemap generated.');
```

---

## 26. Create a basic cronjob-like scheduler

```javascript
const fs = require('fs');

function scheduledTask() {
  const timestamp = new Date().toISOString();
  fs.writeFileSync(`./log_${timestamp}.txt`, 'Scheduled task executed.');
  console.log('Task executed at:', timestamp);
}

setInterval(scheduledTask, 3600000); // Runs every hour
```

---

## 27. Create a \"Safe Delete\" script with user confirmation

```javascript
const fs = require('fs');
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

const fileToDelete = './important.txt';

if (fs.existsSync(fileToDelete)) {
  rl.question(`Are you sure you want to delete ${fileToDelete}? (yes/no) `, (answer) => {
    if (answer.toLowerCase() === 'yes') {
      fs.unlinkSync(fileToDelete);
      console.log('File deleted.');
    } else {
      console.log('Deletion cancelled.');
    }
    rl.close();
  });
} else {
  console.log('File not found.');
  rl.close();
}
```

---

## 28. Create a file integrity checker (SHA256)

```javascript
const fs = require('fs');
const crypto = require('crypto');

function getFileHash(filePath) {
  const fileBuffer = fs.readFileSync(filePath);
  const hashSum = crypto.createHash('sha256');
  hashSum.update(fileBuffer);
  return hashSum.digest('hex');
}

const file = './important.txt';
const expectedHash = 'your_expected_hash_here';

const actualHash = getFileHash(file);

if (actualHash === expectedHash) {
  console.log('File integrity verified.');
} else {
  console.log('WARNING: File has been altered!');
}
```

---

## 29. Mirror directory structure without copying files

```javascript
const fs = require('fs');
const path = require('path');

const source = './source_project';
const destination = './destination_structure';

function mirrorStructure(src, dest) {
  fs.readdirSync(src).forEach(item => {
    const srcPath = path.join(src, item);
    const destPath = path.join(dest, item);

    if (fs.statSync(srcPath).isDirectory()) {
      if (!fs.existsSync(destPath)) {
        fs.mkdirSync(destPath, { recursive: true });
        console.log('Created:', destPath);
      }
      mirrorStructure(srcPath, destPath);
    }
  });
}

mirrorStructure(source, destination);
```

---

## 30. Detect and remove broken symbolic links

```javascript
const fs = require('fs');
const path = require('path');

const directory = './';

fs.readdir(directory, (err, files) => {
  files.forEach(file => {
    const fullPath = path.join(directory, file);
    if (fs.lstatSync(fullPath).isSymbolicLink()) {
      const target = fs.readlinkSync(fullPath);
      if (!fs.existsSync(target)) {
        fs.unlinkSync(fullPath);
        console.log('Removed broken symlink:', file);
      }
    }
  });
});
```

---
