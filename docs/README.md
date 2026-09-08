# 🛡️ Cybersecurity Writeups & Reports Template

Plantilla modular en $\LaTeX$ diseñada para documentar la resolución de máquinas vulnerables, laboratorios web y desafíos CTF, tanto en **reportes individuales** como en un **documento maestro recopilatorio (Knowledge Base / Playbook)**.

---

## 📁 Estructura del Módulo

```text
cybersecurity/
├── style/
│   └── cybersec.sty               # Paquete principal con estilos, comandos, cajas y badges
├── figure/                        # Carpeta raíz para imágenes y logos globales
├── main_compilation.tex           # Documento maestro (Recopilatorio con índice y capítulos)
├── README.md                      # Documentación completa y comandos de compilación
├── chapters/
│   ├── guide/
│   │   └── template_guide.tex     # Manual de uso técnico
│   └── knowledge_base/
│       └── knowledge_base.tex     # Base de conocimiento transversal ("Qué aprendí y Dónde")
└── writeups/                      # Casos organizados por plataforma
    ├── hackthebox/
    │   ├── _template_htb.tex      # Plantilla rápida para nuevas máquinas HTB
    │   └── lame/
    │       ├── writeup.tex        # Caso de ejemplo completo y funcional (Lame)
    │       └── figure/            # Subcarpeta para capturas/evidencias del caso
    ├── tryhackme/
    │   ├── _template_thm.tex      # Plantilla rápida para salas de THM
    │   └── figure/
    ├── portswigger/
    │   ├── _template_portswigger.tex # Plantilla rápida para laboratorios Web Security
    │   └── figure/
    └── ctf/
        ├── _template_ctf.tex      # Plantilla rápida para desafíos CTF
        └── figure/
```

---

## Cabeceras y Pies de Página Dinámicos

Las páginas se adaptan automáticamente a cada máquina o reto que estés resolviendo:

| Ubicación | Contenido Dinámico | Cómo se configura |
| :--- | :--- | :--- |
| **Cabecera Izquierda** | **Ícono + Nombre de Plataforma** (ej. Ícono de HTB, THM, PortSwigger, CTF o imagen personalizada) | Automático vía `\targetSummary` o manual con `\setCustomHeader` |
| **Cabecera Derecha** | **Nombre de la Máquina / Reto** (ej. `Lame`, `Nibbles`) | Toma el nombre definido en `\targetSummary` |
| **Pie Izquierdo** | **Fecha de Resolución** ingresada manualmente (ej. `17/08/2026`) | Campo fecha de `\targetSummary` |
| **Pie Centro** | **Tu Alias:** `errantprogrammer` | Preconfigurado en `cybersec.sty` |
| **Pie Derecho** | **Número de Página:** `Página X / Y` | Automático con `lastpage` |

### Usar un ícono personalizado (por ejemplo en un nuevo CTF)

Puedes pasar una imagen directamente como plataforma en `\targetSummary` o usar `\setCustomHeader`:

```latex
% Opción 1: Pasar la ruta de la imagen en \targetSummary
\targetSummary{MiReto}{10.10.x.x}{figure/picoctf_logo.png}{Medium}{Linux}{Crypto}{17/08/2026}{flag}{}
```

---

## 📦 Requisitos Previos (Pygments vía uv)

El template utiliza el motor **`minted`** para el resaltado de código y consolas de terminal. Se recomienda instalar Pygments utilizando **`uv`**:

```bash
# Instalar Pygments globalmente con uv
uv tool install pygments

# O en tu entorno Python con uv
uv pip install --system pygments
```

---

## 🚀 Guía de Compilación (con `-shell-escape`)

### 1. Compilar el Documento Maestro (Recopilatorio Completo)

Genera el libro completo con portada, dashboard de máquinas resueltas, índice general y todos los writeups concatenados:

```bash
cd cybersecurity
pdflatex -shell-escape main_compilation.tex
pdflatex -shell-escape main_compilation.tex   # Segunda pasada para actualizar índice y referencias
```

### 2. Compilar un Reporte Individual (Modo Standalone / Subfile)

Cada archivo `writeup.tex` dentro de `writeups/` puede compilarse directamente de forma autónoma gracias al paquete `subfiles`. La numeración y las cabeceras se adaptan automáticamente:

```bash
cd cybersecurity/writeups/hackthebox/lame
pdflatex -shell-escape writeup.tex
```

---

## 🧩 Componentes y Comandos Disponibles

### 1. Ficha Resumen del Objetivo (`\targetSummary`)

```latex
\targetSummary%
  {Nombre_Objetivo}% Nombre de la máquina/sala (Cabecera Derecha)
  {10.10.10.3}% IP o Host
  {HackTheBox}% Plataforma (HackTheBox, TryHackMe, PortSwigger, CTF o ruta de imagen)
  {Easy}% Dificultad (Easy, Medium, Hard, Insane)
  {Linux}% Sistema Operativo (Linux, Windows, Android)
  {Samba / RCE}% Categoría o Tags clave
  {17/08/2026}% Fecha manual (Pie Izquierdo)
  {user_flag_hash}% User Flag
  {root_flag_hash}% Root Flag
```

### 2. Badges y Etiquetas

- **Dificultad:** `\difficultyBadge{Easy}`, `\difficultyBadge{Medium}`, `\difficultyBadge{Hard}`, `\difficultyBadge{Insane}`
- **Severidad:** `\severityBadge{Critical}`, `\severityBadge{High}`, `\severityBadge{Medium}`, `\severityBadge{Low}`, `\severityBadge{Info}`
- **Plataformas:** `\platformBadge{HackTheBox}`, `\platformBadge{TryHackMe}`, `\platformBadge{PortSwigger}`, `\platformBadge{CTF}`
- **Sistemas Operativos:** `\osBadge{Linux}`, `\osBadge{Windows}`, `\osBadge{Android}`

### 3. Cajas de Terminal y Código

- **Terminal interactiva con botones (soporta `#`, `_`, `$`, `/`, pipes sin escapar):**

  ```latex
  \begin{terminal}[kali@pentest:~#]
  nmap -p- --open -sS 10.10.10.3 -oN nmap.txt
  \end{terminal}
  ```

- **Bloque de código / exploit:**

  ```latex
  \begin{codebox}[Python]{exploit.py}
  import requests
  # Tu payload / solver script
  \end{codebox}
  ```

### 4. Banderas y Vulnerabilidades

- **Caja de Flag capturada:**

  ```latex
  \begin{flagbox}{User Flag}{92c5a1b32d04e5781a2f4c6e8b1d3fa1}
  \end{flagbox}
  ```

- **Caja de Vulnerabilidad & CVSS:**

  ```latex
  \begin{vulnbox}{Inyección de Comandos en Samba}{Critical}
    \textbf{CVE:} CVE-2007-2447 \hfill \textbf{CVSS v3.1:} 9.8 (Crítico)\\
    \textbf{Impacto:} Ejecución de código como root sin autenticación.
  \end{vulnbox}
  ```

- **Alertas y Mitigaciones:**

  ```latex
  \begin{notebox}[Nota de Enumeración]
    Detalle importante encontrado en el código o cabecera HTTP.
  \end{notebox}

  \begin{mitigationbox}[Plan de Remediación]
    Pasos técnicos recomendados para corregir la falla.
  \end{mitigationbox}
  ```

- **Bitácora de Aprendizaje y Fuentes de Consulta:**

  ```latex
  \begin{learningbox}[Concepto Clave Asimilado]
    Detalle técnico, mecanismo de funcionamiento o truco aprendido.
  \end{learningbox}

  \begin{sourcebox}[Bibliografía y Enlaces de Estudio]
    \begin{itemize}[leftmargin=*]
      \resourceItem{Libro}{The Linux Command Line (William Shotts)}{Capítulo 24: Redirección y subshells}
      \resourceItem{Web}{ExplainShell Parser}{\url{https://explainshell.com}}
      \resourceItem{Docs}{GNU Bash Manual}{\url{https://www.gnu.org/software/bash/}}
      \resourceItem{Writeup}{\hyperref[sec:htb-lame]{HTB - Lame}}{Máquina donde se practicó el vector}
    \end{itemize}
  \end{sourcebox}
  ```

- **Insignias de Fuentes (`\sourceBadge`):** `\sourceBadge{Libro}`, `\sourceBadge{Web}`, `\sourceBadge{Docs}`, `\sourceBadge{Curso}`, `\sourceBadge{Lab}`, `\sourceBadge{Writeup}`.

---

## ➕ Cómo agregar un nuevo Writeup al Recopilatorio

1. **Crear una carpeta para el reto:**
   Por ejemplo: `mkdir -p cybersecurity/writeups/hackthebox/mi_nueva_maquina/figure`
2. **Copiar la plantilla correspondiente:**
   `cp cybersecurity/writeups/hackthebox/_template_htb.tex cybersecurity/writeups/hackthebox/mi_nueva_maquina/writeup.tex`
3. **Escribir el contenido del caso:**
   Edita `writeup.tex` y coloca cualquier captura de pantalla dentro de su carpeta `figure/`.
4. **Vincular en `main_compilation.tex`:**
   Abre [main_compilation.tex](file:///home/errantprogrammer/Documents/gitErrant/tex_template/cybersecurity/main_compilation.tex) y agrega la línea en el capítulo correspondiente:

   ```latex
   \subfile{writeups/hackthebox/mi_nueva_maquina/writeup}
   ```

5. **(Opcional) Actualizar la tabla de control / Dashboard** en la sección inicial de `main_compilation.tex`.
