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
	@texfot $(PDFLATEX) main_compilation.tex
	@echo "[*] Compilando Documento Maestro (Pasada 2/2 para índices y dashboard)..."
	@texfot $(PDFLATEX) main_compilation.tex
	@echo "[+] Compilación exitosa: main_compilation.pdf"

# ================================================================================
# Capítulos de Aprendizaje y Estudio Individual (Knowledge Base)
# ================================================================================
kb learn knowledge:
	@echo "[*] Compilando Capítulo de Knowledge Base / Lo Aprendido..."
	@cd chapters/knowledge_base && mkdir -p _minted && texfot $(PDFLATEX) knowledge_base.tex
	@echo "[+] Knowledge Base generada: chapters/knowledge_base/knowledge_base.pdf"

# ================================================================================
# Documentación del Template y Manual Técnico
# ================================================================================
guide docs:
	@echo "[*] Compilando Guía / Manual Técnico de Documentación..."
	@cd chapters/guide && mkdir -p _minted && texfot $(PDFLATEX) template_guide.tex
	@echo "[+] Guía generada: chapters/guide/template_guide.pdf"

# Compilar todos los capítulos bajo chapters/ (guide + knowledge_base)
chapters:
	@for f in $(CHAPTERS); do \
		dir=$$(dirname $$f); \
		base=$$(basename $$f); \
		echo "[*] Compilando capítulo: $$f"; \
		(cd $$dir && mkdir -p _minted && texfot $(PDFLATEX) $$base); \
	done
	@echo "[+] Todos los capítulos han sido compilados exitosamente."

# ================================================================================
# Compilación de Writeups Individuales
# ================================================================================
writeups:
	@echo "[*] Buscando y compilando todos los writeups..."
	@FILES="$(WRITEUPS)"; \
	if [ -z "$$FILES" ]; then \
		FILES=$$(find . -type f -name "writeup.tex"); \
	fi; \
	if [ -z "$$FILES" ]; then \
		echo "[!] No se encontraron writeups para compilar."; \
		exit 1; \
	fi; \
	for f in $$FILES; do \
		dir=$$(dirname "$$f"); \
		base=$$(basename "$$f"); \
		echo "[*] Compilando writeup individual: $$f"; \
		(cd "$$dir" && mkdir -p _minted && texfor $(PDFLATEX) "$$base"); \
	done
	@echo "[+] Todos los writeups individuales han sido compilados exitosamente."

writeup:
	@if [ -z "$(TARGET)" ] && [ -z "$(NAME)" ]; then \
		echo "[!] Error: Especifica la máquina o reto con TARGET=nombre o NAME=nombre"; \
		echo "    Ejemplo: make writeup TARGET=lame"; \
		echo "             make writeup TARGET=pico_format_string"; \
		exit 1; \
	fi
	@tgt=$${TARGET:-$(NAME)}; \
	echo "[*] Buscando writeup para: $$tgt..."; \
	file=$$(find . -type f -name "writeup.tex" | grep "/$$tgt/" | head -n 1); \
	if [ -z "$$file" ]; then \
		echo "[!] Error: No se encontró writeup.tex para el objetivo: $$tgt"; \
		exit 1; \
	fi; \
	dir=$$(dirname "$$file"); \
	echo "[*] Archivo encontrado en: $$file"; \
	echo "[*] Compilando: writeup.tex..."; \
	(cd "$$dir" && mkdir -p _minted && texfot $(PDFLATEX) writeup.tex); \
	echo "[+] Writeup generado exitosamente en: $$dir"

file:
	@if [ -z "$(FILE)" ]; then \
		echo "[!] Error: Especifica el archivo con FILE=nombre (ej: make file FILE=computer101)"; \
		exit 1; \
	fi
	@echo "[*] Buscando $(FILE).tex..."
	@FOUND=$$(find . -type f -name "$(FILE).tex" | head -n 1); \
	if [ -z "$$FOUND" ]; then \
		echo "[!] Error: No se encontró $(FILE).tex en ninguna carpeta."; \
		exit 1; \
	fi; \
	dir=$$(dirname "$$FOUND"); \
	base=$$(basename "$$FOUND"); \
	echo "[*] Archivo encontrado en: $$FOUND"; \
	echo "[*] Compilando: $$base..."; \
	(cd "$$dir" && mkdir -p _minted && texfot $(PDFLATEX) "$$base"); \
	echo "[+] Generado exitosamente en: $$dir"
# ================================================================================
# Modos de Visualización en Vivo y Limpieza
# ================================================================================
watch:
	@if [ -n "$(FILE)" ]; then \
		FOUND=$$(find . -type f -name "$(FILE).tex" | head -n 1); \
		if [ -z "$$FOUND" ]; then \
			echo "[!] Error: No se encontró $(FILE).tex en ninguna carpeta."; \
			exit 1; \
		fi; \
		dir=$$(dirname "$$FOUND"); \
		base=$$(basename "$$FOUND"); \
		echo "[*] Modo continuo activado para: $$FOUND"; \
		(cd "$$dir" && mkdir -p _minted && latexmk -pvc -pdf -pdflatex="$(PDFLATEX)" "$$base"); \
	else \
		mkdir -p _minted; \
		echo "[*] Modo continuo activado para: main_compilation.tex"; \
		latexmk -pvc -pdf -pdflatex="$(PDFLATEX)" main_compilation.tex; \
	fi

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
	@echo "  make writeups            -> Compila automáticamente TODOS los writeups individuales"
	@echo "  make writeup TARGET=...  -> Compila un writeup por nombre (ej: make writeup TARGET=lame)"
	@echo "  make file FILE=nombre    -> Compila el archivo 'nombre.tex' en cualquier subcarpeta."
	@echo "  make watch               -> Modo continuo en vivo (compila al guardar en el editor)"
	@echo "  make clean               -> Elimina todos los archivos auxiliares (.aux, .log, _minted)"
	@echo "================================================================================="
