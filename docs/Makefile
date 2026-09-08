PDFLATEX = pdflatex -synctex=1 -interaction=nonstopmode -shell-escape
LATEXMK  = latexmk -pdf -pdflatex="$(PDFLATEX)"

WRITEUPS = $(wildcard writeups/*/*/writeup.tex)
CHAPTERS = $(wildcard chapters/*/*.tex)

.PHONY: all master kb learn knowledge guide docs chapters chapter writeups writeup file watch clean help

# ================================================================================
# Meta Principal: Compilar el Documento Maestro (Libro Completo)
# ================================================================================
all: master

master:
	@mkdir -p _minted
	@echo "[*] Compilando Documento Maestro (Pasada 1/2)..."
	@$(PDFLATEX) main_compilation.tex
	@echo "[*] Compilando Documento Maestro (Pasada 2/2 para índices y dashboard)..."
	@$(PDFLATEX) main_compilation.tex
	@echo "[+] Compilación exitosa: main_compilation.pdf"

# ================================================================================
# Capítulos de Aprendizaje y Estudio Individual (Knowledge Base)
# ================================================================================
kb learn knowledge:
	@echo "[*] Compilando Capítulo de Knowledge Base / Lo Aprendido..."
	@cd chapters/knowledge_base && mkdir -p _minted && $(PDFLATEX) knowledge_base.tex
	@echo "[+] Knowledge Base generada: chapters/knowledge_base/knowledge_base.pdf"

# ================================================================================
# Documentación del Template y Manual Técnico
# ================================================================================
guide docs:
	@echo "[*] Compilando Guía / Manual Técnico de Documentación..."
	@cd chapters/guide && mkdir -p _minted && $(PDFLATEX) template_guide.tex
	@echo "[+] Guía generada: chapters/guide/template_guide.pdf"

# Compilar todos los capítulos bajo chapters/ (guide + knowledge_base)
chapters:
	@for f in $(CHAPTERS); do \
		dir=$$(dirname $$f); \
		base=$$(basename $$f); \
		echo "[*] Compilando capítulo: $$f"; \
		(cd $$dir && mkdir -p _minted && $(PDFLATEX) $$base); \
	done
	@echo "[+] Todos los capítulos han sido compilados exitosamente."

# Compilar un capítulo genérico pasando NAME=... (ej: make chapter NAME=knowledge_base)
chapter:
	@if [ -z "$(NAME)" ]; then \
		echo "[!] Error: Especifica el nombre de la carpeta con NAME=..."; \
		echo "    Ejemplos: make chapter NAME=knowledge_base"; \
		echo "              make chapter NAME=guide"; \
		exit 1; \
	fi
	@file=$$(find chapters/$(NAME) -maxdepth 1 -name "*.tex" 2>/dev/null | head -n 1); \
	if [ -z "$$file" ]; then \
		echo "[!] No se encontró ningún archivo .tex en chapters/$(NAME)"; \
		exit 1; \
	fi; \
	dir=$$(dirname $$file); \
	base=$$(basename $$file); \
	echo "[*] Compilando capítulo: $$file..."; \
	(cd $$dir && mkdir -p _minted && $(PDFLATEX) $$base); \
	echo "[+] Capítulo generado exitosamente en: $$dir"

# ================================================================================
# Compilación de Writeups Individuales
# ================================================================================
writeups:
	@for f in $(WRITEUPS); do \
		dir=$$(dirname $$f); \
		base=$$(basename $$f); \
		echo "[*] Compilando writeup individual: $$f"; \
		(cd $$dir && mkdir -p _minted && $(PDFLATEX) $$base); \
	done
	@echo "[+] Todos los writeups individuales han sido compilados exitosamente."

writeup:
	@if [ -z "$(TARGET)" ] && [ -z "$(NAME)" ]; then \
		echo "[!] Especifica la máquina o reto con TARGET=nombre"; \
		echo "    Ejemplo: make writeup TARGET=lame"; \
		echo "             make writeup TARGET=pico_format_string"; \
		exit 1; \
	fi
	@tgt=$${TARGET:-$(NAME)}; \
	file=$$(find writeups -type f -name "writeup.tex" | grep "/$$tgt/" | head -n 1); \
	if [ -z "$$file" ]; then \
		echo "[!] No se encontró writeup para el objetivo: $$tgt"; \
		exit 1; \
	fi; \
	dir=$$(dirname $$file); \
	echo "[*] Compilando writeup: $$file..."; \
	(cd $$dir && mkdir -p _minted && $(PDFLATEX) writeup.tex); \
	echo "[+] Writeup generado exitosamente en: $$dir"

file:
	@if [ -z "$(FILE)" ]; then \
		echo "[!] Error: Especifica el archivo con FILE=ruta/al/archivo.tex"; \
		exit 1; \
	fi
	@dir=$$(dirname $(FILE)); \
	base=$$(basename $(FILE)); \
	echo "[*] Compilando: $(FILE)..."; \
	(cd $$dir && mkdir -p _minted && $(PDFLATEX) $$base); \
	echo "[+] Generado exitosamente en: $$dir"

# ================================================================================
# Modos de Visualización en Vivo y Limpieza
# ================================================================================
watch:
	@mkdir -p _minted
	latexmk -pvc -pdf -pdflatex="$(PDFLATEX)" main_compilation.tex

clean:
	@echo "[*] Limpiando archivos temporales en todo el proyecto..."
	@find . -type f \( -name "*.aux" -o -name "*.log" -o -name "*.toc" -o -name "*.synctex.gz" \
	       -o -name "*.out" -o -name "*.targets" -o -name "*.listing" -o -name "*.fls" \
	       -o -name "*.fdb_latexmk" \) -delete 2>/dev/null || true
	@find . -type d -name "_minted*" -exec rm -rf {} + 2>/dev/null || true
	@echo "[+] Limpieza completada."

help:
	@echo "================================================================================="
	@echo "                   🛡️  CYBERSECURITY PLAYBOOK & WRITEUPS                        "
	@echo "================================================================================="
	@echo "  make                     -> Compila el libro maestro completo (main_compilation.pdf)"
	@echo "  make kb                  -> Compila SOLO el capítulo de lo aprendido (Knowledge Base)"
	@echo "  make docs (o make guide) -> Compila la guía técnica (chapters/guide/template_guide.pdf)"
	@echo "  make chapters            -> Compila todos los capítulos de documentación y aprendizaje"
	@echo "  make chapter NAME=...    -> Compila un capítulo por nombre (ej: make chapter NAME=knowledge_base)"
	@echo "  make writeups            -> Compila automáticamente TODOS los writeups individuales"
	@echo "  make writeup TARGET=...  -> Compila un writeup por nombre (ej: make writeup TARGET=lame)"
	@echo "  make file FILE=...       -> Compila un archivo específico por ruta exacta"
	@echo "  make watch               -> Modo continuo en vivo (compila al guardar en el editor)"
	@echo "  make clean               -> Elimina todos los archivos auxiliares (.aux, .log, _minted)"
	@echo "================================================================================="
