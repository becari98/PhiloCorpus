# PhiloCorpus (T420 Edition)

## Guia Completo de Instalação, Configuração e Execução

**Versão:** Última Implementação de Produção
**Sistema recomendado:** Linux Mint 22.3 Xfce (64 bits)
**Hardware-alvo:** Lenovo ThinkPad T420 (i7-2760QM, 16 GB RAM)

---

# 1. Visão Geral

O **PhiloCorpus** é uma plataforma local de pesquisa filosófica que integra:

* Biblioteca acadêmica estruturada em Markdown;
* OCR de livros e artigos em PDF;
* Banco relacional SQLite;
* Banco vetorial LanceDB;
* Recuperação híbrida (RAG);
* Pesquisa dialogada com LLM local via Ollama;
* Geração de referências ABNT;
* Coleta bibliográfica em bases acadêmicas;
* Interface gráfica em PyQt5.

---

# 2. Arquitetura Geral

```text
PDF
 ↓
OCR / Extração de Texto
 ↓
Markdown Estruturado
 ↓
SQLite + FTS5
 ↓
Embeddings
 ↓
LanceDB
 ↓
RAG Híbrido
 ↓
LLM Local (Ollama)
 ↓
Resposta Filosófica Contextualizada
```

---

# 3. Estrutura do Projeto

```text
PhiloCorpus/
│
├── app.py
├── requirements.txt
├── philocorpus/
│
├── config.py
├── database.py
├── gui.py
│
├── services/
│   ├── pdf_converter.py
│   ├── markdown_processor.py
│   ├── embedding_rag.py
│   ├── llm_router.py
│   └── abnt_formatter.py
│
├── plugins/
│   └── scrapers.py
│
└── __init__.py
```

---

# 4. Instalação das Dependências do Sistema

Atualize o sistema:

```bash
sudo apt update
sudo apt upgrade
```

Instale os pacotes necessários:

```bash
sudo apt install -y \
python3 \
python3-pip \
python3-venv \
build-essential \
sqlite3 \
tesseract-ocr \
tesseract-ocr-por \
tesseract-ocr-eng \
ocrmypdf \
ghostscript \
poppler-utils \
libgl1 \
libglib2.0-0 \
libsqlite3-dev
```

Verifique o Tesseract:

```bash
tesseract --version
```

Verifique o SQLite:

```bash
sqlite3 --version
```

Verifique o suporte ao FTS5:

```bash
sqlite3
```

Dentro do prompt:

```sql
pragma compile_options;
```

Procure:

```text
ENABLE_FTS5
```

Caso não apareça, o sistema continuará funcionando normalmente, apenas sem busca lexical FTS5.

---

# 5. Criação do Ambiente Virtual

Entre na pasta do projeto:

```bash
cd ~/Projetos
mkdir PhiloCorpus
cd PhiloCorpus
```

Crie o ambiente:

```bash
python3 -m venv .venv
```

Ative:

```bash
source .venv/bin/activate
```

Atualize o pip:

```bash
pip install --upgrade pip wheel setuptools
```

---

# 6. Arquivo requirements.txt

Crie:

```text
PyQt5
PyMuPDF
Pillow
PyYAML
numpy
pyarrow
lancedb
sentence-transformers
transformers
torch
httpx
beautifulsoup4
pytesseract
ocrmypdf
requests
```

Instale:

```bash
pip install -r requirements.txt
```

---

# 7. Instalação do Ollama

Instale:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Verifique:

```bash
ollama --version
```

Inicie:

```bash
systemctl --user start ollama
```

ou:

```bash
ollama serve
```

---

# 8. Instalação dos Modelos Locais

## Modelo leve (recomendado)

```bash
ollama pull deepseek-r1:1.5b
```

## Alternativas

```bash
ollama pull qwen2.5:3b
```

```bash
ollama pull qwen2.5:7b
```

```bash
ollama pull mistral:7b
```

Para o T420, recomenda-se:

```text
deepseek-r1:1.5b
```

ou

```text
qwen2.5:3b
```

---

# 9. Download do Modelo de Embeddings

O primeiro processamento de documento fará o download automático:

```text
intfloat/multilingual-e5-small
```

Também pode ser pré-baixado:

```bash
python
```

```python
from sentence_transformers import SentenceTransformer
SentenceTransformer("intfloat/multilingual-e5-small")
```

---

# 10. Estrutura de Diretórios Gerada

Na primeira execução serão criadas:

```text
~/.config/philocorpus/
    settings.yaml

~/.local/share/philocorpus/logs/
    app.log

~/PhiloCorpus/
    Biblioteca/
    cache/
    exports/
    philocorpus.db
    lancedb/
```

---

# 11. Configuração do settings.yaml

Exemplo:

```yaml
paths:
  library: /home/usuario/PhiloCorpus/Biblioteca
  cache: /home/usuario/PhiloCorpus/cache
  exports: /home/usuario/PhiloCorpus/exports
  db: /home/usuario/PhiloCorpus/philocorpus.db
  lancedb: /home/usuario/PhiloCorpus/lancedb

ocr:
  engine: tesseract
  default_lang: por

embeddings:
  model: intfloat/multilingual-e5-small
  dimension: 384

llm:
  provider: ollama
  local_model: deepseek-r1:1.5b
```

---

# 12. Organização dos Arquivos Fonte

Estrutura:

```text
PhiloCorpus/
│
├── app.py
├── requirements.txt
└── philocorpus/
```

Copie os arquivos da última implementação exatamente para essa estrutura.

---

# 13. Execução

Ative o ambiente:

```bash
source .venv/bin/activate
```

Inicie o Ollama:

```bash
ollama serve
```

Abra outro terminal.

Execute:

```bash
python app.py
```

---

# 14. Primeiro Uso

## Etapa 1

Abra:

```text
Esteira Ingestão
```

---

## Etapa 2

Selecione:

```text
Selecionar PDF e Iniciar Ingestão
```

---

## Etapa 3

O sistema executará:

1. análise do PDF;
2. OCR, se necessário;
3. extração de metadados;
4. criação de Markdown;
5. indexação SQLite;
6. criação dos embeddings;
7. indexação no LanceDB.

---

## Etapa 4

O documento aparecerá em:

```text
Biblioteca
```

---

## Etapa 5

Ative a obra em:

```text
Pesquisa Dialogada
```

Exemplos:

```text
Como Nietzsche concebe o homem como animal?
```

```text
Quais passagens tratam do homo natura?
```

```text
Onde aparece a crítica ao antropocentrismo?
```

---

# 15. Logs

Logs persistentes:

```text
~/.local/share/philocorpus/logs/app.log
```

Monitoramento em tempo real:

```bash
tail -f ~/.local/share/philocorpus/logs/app.log
```

---

# 16. Backup

Banco:

```text
~/PhiloCorpus/philocorpus.db
```

Vetores:

```text
~/PhiloCorpus/lancedb/
```

Biblioteca:

```text
~/PhiloCorpus/Biblioteca/
```

Backup:

```bash
tar -czf philocorpus-backup.tar.gz ~/PhiloCorpus
```

---

# 17. Recuperação

Apague apenas o índice vetorial:

```bash
rm -rf ~/PhiloCorpus/lancedb
```

Na próxima indexação ele será reconstruído automaticamente.

---

# 18. Encerramento

Feche a aplicação normalmente.

Se necessário:

```bash
pkill -f app.py
```

Pare o Ollama:

```bash
pkill ollama
```

---

# Fluxo Completo de Pesquisa Filosófica

```text
PDF
 ↓
OCR
 ↓
Markdown
 ↓
SQLite
 ↓
Embeddings
 ↓
LanceDB
 ↓
RAG Híbrido
 ↓
LLM Local
 ↓
Resposta Filosófica com Contexto e Paginação
```

O sistema está pronto para operar como uma plataforma local de pesquisa filosófica, análise textual e construção de corpus acadêmico em ambiente Linux.

---

### 1. `philocorpus/config.py`
* **Melhorias**:
  * Implementação de um método **Deep Merge** robusto para evitar falhas `TypeError: 'NoneType' object is not subscriptable` quando arquivos YAML estiverem parcialmente desconfigurados ou vazios (**Correção 5**).
  * Criação automática de logs rotativos e persistentes salvos no caminho acadêmico padrão do usuário (`~/.local/share/philocorpus/logs/app.log`) (**Correção 12**).

```python
import os
import yaml
import logging
from pathlib import Path
from logging.handlers import RotatingFileHandler

DEFAULT_CONFIG = {
    "paths": {
        "library": str(Path.home() / "PhiloCorpus" / "Biblioteca"),
        "cache": str(Path.home() / "PhiloCorpus" / "cache"),
        "exports": str(Path.home() / "PhiloCorpus" / "exports"),
        "db": str(Path.home() / "PhiloCorpus" / "philocorpus.db"),
        "lancedb": str(Path.home() / "PhiloCorpus" / "lancedb")
    },
    "ocr": {
        "engine": "tesseract",  # 'tesseract' ou 'ocrmypdf'
        "default_lang": "por"
    },
    "embeddings": {
        "model": "intfloat/multilingual-e5-small",
        "dimension": 384
    },
    "llm": {
        "provider": "ollama",
        "local_model": "deepseek-r1:1.5b",
        "remote_endpoint": "https://api.openai.com/v1",
        "remote_key": "",
        "remote_model": "deepseek-chat",
        "gemini_key": ""
    },
    "rag": {
        "top_k": 5
    }
}

def deep_merge(dict1, dict2):
    """
    Correção 5: Mescla dicionários recursivamente garantindo que chaves nulas do arquivo
    YAML quebrado sejam preenchidas com as configurações padrões de segurança.
    """
    result = dict1.copy()
    for key, val in dict2.items():
        if isinstance(val, dict) and key in result and isinstance(result[key], dict):
            result[key] = deep_merge(result[key], val)
        elif val is not None:
            result[key] = val
    return result

class ConfigManager:
    def __init__(self):
        self.config_dir = Path.home() / ".config" / "philocorpus"
        self.config_dir.mkdir(parents=True, exist_ok=True)
        self.config_path = self.config_dir / "settings.yaml"
        self.settings = self.load_config()
        self.initialize_dirs()
        self.configure_logging()

    def load_config(self):
        if not self.config_path.exists():
            with open(self.config_path, "w", encoding="utf-8") as f:
                yaml.dump(DEFAULT_CONFIG, f, default_flow_style=False)
            return DEFAULT_CONFIG
        try:
            with open(self.config_path, "r", encoding="utf-8") as f:
                # Correção 5: Validação defensiva do arquivo carregado
                cfg = yaml.safe_load(f) or {}
                return deep_merge(DEFAULT_CONFIG, cfg)
        except Exception:
            return DEFAULT_CONFIG

    def save_config(self):
        with open(self.config_path, "w", encoding="utf-8") as f:
            yaml.dump(self.settings, f, default_flow_style=False)

    def initialize_dirs(self):
        for path_name, path_str in self.settings["paths"].items():
            if path_name == "db":
                Path(path_str).parent.mkdir(parents=True, exist_ok=True)
            else:
                Path(path_str).mkdir(parents=True, exist_ok=True)

    def configure_logging(self):
        """
        Correção 12: Configura a persistência rotativa de logs acadêmicos locais.
        """
        log_dir = Path.home() / ".local" / "share" / "philocorpus" / "logs"
        log_dir.mkdir(parents=True, exist_ok=True)
        log_file = log_dir / "app.log"
        
        logger = logging.getLogger()
        logger.setLevel(logging.INFO)
        
        formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
        
        file_handler = RotatingFileHandler(log_file, maxBytes=5*1024*1024, backupCount=3)
        file_handler.setFormatter(formatter)
        logger.addHandler(file_handler)
        
        stream_handler = logging.StreamHandler()
        stream_handler.setFormatter(formatter)
        logger.addHandler(stream_handler)

config_manager = ConfigManager()
```

---

### 2. `philocorpus/database.py`
* **Melhorias**:
  * Tratamento de concorrência robusto de threads ao instanciar conexões SQLite no T420 por meio do ajuste do `timeout=30` e injeção do comando de bloqueio ativo `PRAGMA busy_timeout = 30000;` (**Correção 10**).
  * Isolamento defensivo completo das operações do `fts_enabled` para garantir integridade caso o sistema não possua o módulo SQLite FTS5 pré-compilado.

```python
import sqlite3
import logging

logger = logging.getLogger("philocorpus.database")

class DatabaseManager:
    def __init__(self):
        from philocorpus.config import config_manager
        self.db_path = config_manager.settings["paths"]["db"]
        self.fts_enabled = True
        self.init_db()

    def get_connection(self):
        # Correção 10: Adição preventiva de timeout de 30 segundos nas conexões
        conn = sqlite3.connect(self.db_path, timeout=30.0)
        conn.row_factory = sqlite3.Row
        
        # Correção 10: Tratamento preventivo contra locks de concorrência concorrentes ("database is locked")
        conn.execute("PRAGMA busy_timeout = 30000;")
        conn.execute("PRAGMA journal_mode=WAL;")
        conn.execute("PRAGMA synchronous=NORMAL;")
        conn.execute("PRAGMA temp_store=MEMORY;")
        conn.execute("PRAGMA foreign_keys=ON;")
        return conn

    def init_db(self):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            
            cursor.execute("""
            CREATE TABLE IF NOT EXISTS authors (
                id TEXT PRIMARY KEY,
                name TEXT NOT NULL UNIQUE,
                birth TEXT,
                death TEXT,
                country TEXT
            )""")

            cursor.execute("""
            CREATE TABLE IF NOT EXISTS documents (
                id TEXT PRIMARY KEY,
                title TEXT NOT NULL,
                subtitle TEXT,
                author_id TEXT,
                year INTEGER,
                language TEXT,
                publisher TEXT,
                isbn TEXT,
                doi TEXT,
                path TEXT,
                FOREIGN KEY(author_id) REFERENCES authors(id)
            )""")

            cursor.execute("""
            CREATE TABLE IF NOT EXISTS chapters (
                id TEXT PRIMARY KEY,
                document_id TEXT,
                title TEXT,
                position INTEGER,
                path TEXT,
                FOREIGN KEY(document_id) REFERENCES documents(id)
            )""")

            cursor.execute("""
            CREATE TABLE IF NOT EXISTS sections (
                id TEXT PRIMARY KEY,
                chapter_id TEXT,
                title TEXT,
                position INTEGER,
                path TEXT,
                FOREIGN KEY(chapter_id) REFERENCES chapters(id)
            )""")

            cursor.execute("""
            CREATE TABLE IF NOT EXISTS chunks (
                id TEXT PRIMARY KEY,
                section_id TEXT,
                text TEXT,
                chunk_type TEXT,
                page_start INTEGER,
                page_end INTEGER,
                position_index INTEGER,
                prev_chunk_id TEXT,
                next_chunk_id TEXT,
                FOREIGN KEY(section_id) REFERENCES sections(id)
            )""")

            cursor.execute("""
            CREATE TABLE IF NOT EXISTS notes (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                document_id TEXT,
                content TEXT,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                FOREIGN KEY(document_id) REFERENCES documents(id)
            )""")

            cursor.execute("CREATE INDEX IF NOT EXISTS idx_chunks_section ON chunks(section_id);")
            cursor.execute("CREATE INDEX IF NOT EXISTS idx_chunks_position ON chunks(position_index);")
            cursor.execute("CREATE INDEX IF NOT EXISTS idx_documents_author ON documents(author_id);")
            cursor.execute("CREATE INDEX IF NOT EXISTS idx_sections_chapter ON sections(chapter_id);")
            cursor.execute("CREATE INDEX IF NOT EXISTS idx_chapters_document ON chapters(document_id);")

            # Correção de robustez para FTS5
            try:
                cursor.execute("""
                CREATE VIRTUAL TABLE IF NOT EXISTS chunks_fts USING fts5(
                    id UNINDEXED,
                    text
                )""")
                conn.commit()
            except sqlite3.OperationalError:
                self.fts_enabled = False
                logger.warning("FTS5 não está habilitado/compilado no SQLite do sistema. Busca lexical FTS5 desativada.")

    def add_author(self, author_id, name, birth=None, death=None, country=None):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            try:
                cursor.execute(
                    "INSERT INTO authors (id, name, birth, death, country) VALUES (?, ?, ?, ?, ?)",
                    (author_id, name, birth, death, country)
                )
                conn.commit()
                return author_id
            except sqlite3.IntegrityError:
                cursor.execute("SELECT id FROM authors WHERE name = ?", (name,))
                row = cursor.fetchone()
                return row[0] if row else author_id

    def add_document(self, doc_id, title, subtitle, author_id, year, language, publisher, isbn, doi, path):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                INSERT OR REPLACE INTO documents (id, title, subtitle, author_id, year, language, publisher, isbn, doi, path)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            """, (doc_id, title, subtitle, author_id, year, language, publisher, isbn, doi, path))
            conn.commit()

    def add_chapter(self, chap_id, doc_id, title, position, path):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                INSERT OR REPLACE INTO chapters (id, document_id, title, position, path)
                VALUES (?, ?, ?, ?, ?)
            """, (chap_id, doc_id, title, position, path))
            conn.commit()

    def add_section(self, sec_id, chap_id, title, position, path):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                INSERT OR REPLACE INTO sections (id, chapter_id, title, position, path)
                VALUES (?, ?, ?, ?, ?)
            """, (sec_id, chap_id, title, position, path))
            conn.commit()

    def bulk_insert_chunks_and_fts(self, chunks_list):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            try:
                cursor.executemany("""
                    INSERT OR REPLACE INTO chunks 
                    (id, section_id, text, chunk_type, page_start, page_end, position_index, prev_chunk_id, next_chunk_id)
                    VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)
                """, [
                    (c["id"], c["sec_id"], c["text"], c["chunk_type"], c["page_start"], 
                     c["page_end"], c["position_index"], c["prev_chunk_id"], c["next_chunk_id"])
                    for c in chunks_list
                ])
                
                if self.fts_enabled:
                    cursor.executemany("""
                        INSERT OR REPLACE INTO chunks_fts (id, text) VALUES (?, ?)
                    """, [(c["id"], c["text"]) for c in chunks_list])
                
                conn.commit()
            except Exception as e:
                conn.rollback()
                logger.error(f"Erro na transação de escrita de chunks: {e}")
                raise e

    def fts_search(self, term: str, limit: int = 20) -> list[str]:
        if not self.fts_enabled:
            return []
        with self.get_connection() as conn:
            cursor = conn.cursor()
            safe_term = term.replace('"', '""')
            cursor.execute("""
                SELECT id FROM chunks_fts 
                WHERE text MATCH ? LIMIT ?
            """, (f'"{safe_term}"', limit))
            return [row["id"] for row in cursor.fetchall()]

    def get_chunk(self, chunk_id):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                SELECT 
                    chunks.*, 
                    sections.title as sec_title, 
                    chapters.title as chap_title, 
                    documents.id as document_id,
                    documents.title as doc_title
                FROM chunks
                JOIN sections ON chunks.section_id = sections.id
                JOIN chapters ON sections.chapter_id = chapters.id
                JOIN documents ON chapters.document_id = documents.id
                WHERE chunks.id = ?
            """, (chunk_id,))
            return cursor.fetchone()

    def get_notes(self, doc_id):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("SELECT * FROM notes WHERE document_id = ? ORDER BY created_at DESC", (doc_id,))
            return cursor.fetchall()

    def add_note(self, doc_id, content):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("INSERT INTO notes (document_id, content) VALUES (?, ?)", (doc_id, content))
            conn.commit()

db = DatabaseManager()
```

---

### 3. `philocorpus/services/pdf_converter.py`
* **Melhorias**:
  * Uso do gerenciador de contexto `with fitz.open` garantindo o encerramento seguro de recursos físicos, mitigando o risco de locks do sistema operacional ao processar pilhas acadêmicas (**Correção 8**).
  * Exceções no motor OCR de fallback estendidas de forma ampla para capturar erros fatais de ambiente do tesseract, como `TesseractNotFoundError` (**Correção 6**).

```python
import fitz
import re
import random
import subprocess
import logging
import io
from pathlib import Path
from PIL import Image
from philocorpus.config import config_manager

logger = logging.getLogger("philocorpus.pdf_converter")

class PDFConverterService:
    @staticmethod
    def deep_check_text_layer(pdf_path: str) -> bool:
        # Correção 8: Garante o fechamento explícito do PyMuPDF usando bloco 'with'
        try:
            with fitz.open(pdf_path) as doc:
                num_pages = len(doc)
                if num_pages == 0:
                    return False
                    
                pages_to_check = set(range(min(20, num_pages)))
                if num_pages > 20:
                    pages_to_check.update(random.sample(range(20, num_pages), min(10, num_pages - 20)))
                    
                total_chars = 0
                for page_idx in pages_to_check:
                    total_chars += len(doc[page_idx].get_text().strip())
                
                return (total_chars / len(pages_to_check)) > 100
        except Exception as e:
            logger.error(f"Erro na análise estocástica: {e}")
            return False

    @staticmethod
    def run_ocr_mypdf(pdf_path: str, output_path: str) -> bool:
        lang = config_manager.settings["ocr"]["default_lang"]
        try:
            cmd = ["ocrmypdf", "-l", lang, "--skip-text", pdf_path, output_path]
            subprocess.run(cmd, check=True, stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL)
            return True
        except (subprocess.SubprocessError, FileNotFoundError, OSError) as e:
            logger.warning(f"OCRmyPDF indisponível: {e}. Executando fallback automático.")
            return False

    @staticmethod
    def extract_text_via_tesseract_fallback(pdf_path: str) -> str:
        import pytesseract
        lang = config_manager.settings["ocr"]["default_lang"]
        logger.info("Iniciando varredura OCR interna via Tesseract...")
        
        structured_lines = []
        
        # Correção 8: Fechamento explícito via gerenciador de contexto 'with'
        with fitz.open(pdf_path) as doc:
            for page_idx in range(len(doc)):
                page_num = page_idx + 1
                structured_lines.append(f"\n<!-- PAGE_START {page_num} -->\n")
                
                page = doc[page_idx]
                pix = page.get_pixmap(dpi=150)
                img_data = pix.tobytes("png")
                img = Image.open(io.BytesIO(img_data))
                
                # Correção 6: Varredura defensiva estendida para abranger erros do executável ausente (pytesseract.TesseractNotFoundError)
                try:
                    ocr_text = pytesseract.image_to_string(img, lang=lang)
                except Exception as e:
                    logger.warning(f"Erro com idioma '{lang}' ou Tesseract indisponível no sistema: {e}. Forçando fallback para 'eng'.")
                    try:
                        ocr_text = pytesseract.image_to_string(img, lang="eng")
                    except Exception as ex:
                        logger.error(f"Falha total no Tesseract: {ex}")
                        ocr_text = "[Falha na extração por OCR - Instale o Tesseract no sistema]"
                        
                structured_lines.append(ocr_text)
                structured_lines.append(f"\n<!-- PAGE_END {page_num} -->\n")
            
        return "\n".join(structured_lines)

    @classmethod
    def extract_and_structure_pdf(cls, pdf_path: str) -> str:
        has_text = cls.deep_check_text_layer(pdf_path)
        
        if not has_text:
            logger.info("Falta de camada de texto identificada. Iniciando processamento de OCR.")
            ocr_pdf_path = str(Path(config_manager.settings["paths"]["cache"]) / f"ocr_{Path(pdf_path).name}")
            
            success = cls.run_ocr_mypdf(pdf_path, ocr_pdf_path)
            if success:
                pdf_path = ocr_pdf_path
            else:
                return cls.extract_text_via_tesseract_fallback(pdf_path)

        # Correção 8: Garante fechamento correto em caso de exceções utilizando bloco 'with'
        with fitz.open(pdf_path) as doc:
            structured_lines = []
            
            chap_regex = re.compile(
                r"^(\s*)*(CAP[ÍI]TULO|CHAPTER|CHAPITRE|KAPITEL|LIVRO|PARTE|SEÇÃO|BOOK|PART)\s+([IVXLCDM\d]+)", 
                re.IGNORECASE
            )
            aph_regex = re.compile(r"^(\s*)*(AFORISMO|PROPOSIÇÃO|PROPOSITION|§)\s*(\d+)", re.IGNORECASE)
            roman_heading = re.compile(r"^(\s*)*[IVXLCDM]+\.?(\s*)*$")

            for page_idx in range(len(doc)):
                page_num = page_idx + 1
                structured_lines.append(f"\n<!-- PAGE_START {page_num} -->\n")
                page_text = doc[page_idx].get_text("blocks")
                
                for block in page_text:
                    block_text = block[4].strip()
                    lines = block_text.split("\n")
                    for line in lines:
                        line_stripped = line.strip()
                        if not line_stripped:
                            continue
                        
                        if chap_regex.match(line_stripped):
                            structured_lines.append(f"\n# {line_stripped}\n")
                        elif aph_regex.match(line_stripped):
                            structured_lines.append(f"\n## {line_stripped}\n")
                        elif roman_heading.match(line_stripped) and len(line_stripped) < 10:
                            structured_lines.append(f"\n# {line_stripped}\n")
                        else:
                            structured_lines.append(line_stripped)
                structured_lines.append(f"\n<!-- PAGE_END {page_num} -->\n")
                
        return "\n".join(structured_lines)
```

---

### 4. `philocorpus/services/markdown_processor.py`
* Estrutura canônica de arquivos em Markdown.

```python
import uuid
import re
from pathlib import Path
from philocorpus.config import config_manager
from philocorpus.database import db

class MarkdownProcessor:
    @staticmethod
    def generate_slug(text: str) -> str:
        clean = re.sub(r'[^\w\s-]', '', text).strip().lower().replace(' ', '_')
        return clean[:30]

    @classmethod
    def process_and_save(cls, raw_text: str, metadata: dict) -> tuple[str, str]:
        author_slug = cls.generate_slug(metadata.get("author", "Autor_Desconhecido"))
        title_slug = cls.generate_slug(metadata.get("title", "Obra_Sem_Titulo"))
        
        doc_uuid = str(uuid.uuid4())[:8]
        doc_id = f"{author_slug}_{title_slug}_{doc_uuid}"
        
        library_dir = Path(config_manager.settings["paths"]["library"])
        doc_dir = library_dir / author_slug / title_slug
        doc_dir.mkdir(parents=True, exist_ok=True)
        
        md_dir = doc_dir / "markdown"
        chap_dir = md_dir / "chapters"
        sec_dir = md_dir / "sections"
        
        for d in [md_dir, chap_dir, sec_dir]:
            d.mkdir(parents=True, exist_ok=True)

        author_id = f"auth_{author_slug}_{str(uuid.uuid4())[:4]}"
        db.add_author(author_id, metadata.get("author", "Autor Desconhecido"))
        
        db.add_document(
            doc_id=doc_id,
            title=metadata.get("title", "Título Desconhecido"),
            subtitle=metadata.get("subtitle", ""),
            author_id=author_id,
            year=metadata.get("year"),
            language=metadata.get("language", "pt"),
            publisher=metadata.get("publisher", ""),
            isbn=metadata.get("isbn", ""),
            doi=metadata.get("doi", ""),
            path=str(doc_dir)
        )

        frontmatter = (
            "---\n"
            f"id: {doc_id}\n"
            f"title: \"{metadata.get('title')}\"\n"
            f"author: \"{metadata.get('author')}\"\n"
            f"year: {metadata.get('year') or 'null'}\n"
            f"language: \"{metadata.get('language')}\"\n"
            f"isbn: \"{metadata.get('isbn')}\"\n"
            f"doi: \"{metadata.get('doi')}\"\n"
            "---\n\n"
        )
        
        full_md_path = md_dir / "obra.md"
        with open(full_md_path, "w", encoding="utf-8") as f:
            f.write(frontmatter + raw_text)
            
        cls.split_hierarchy(raw_text, doc_id, chap_dir, sec_dir)
        return doc_id, str(full_md_path)

    @classmethod
    def split_hierarchy(cls, raw_text: str, doc_id: str, chap_dir: Path, sec_dir: Path):
        lines = raw_text.split("\n")
        current_chapter_title = "Introdução"
        current_section_title = "Seção Geral"
        
        current_chapter_lines = []
        current_section_lines = []
        
        chap_count = 0
        sec_count = 0
        
        def save_sec(s_title, lines_list):
            nonlocal sec_count
            sec_slug = cls.generate_slug(s_title)
            sec_id = f"{doc_id}_c{chap_count}_s{sec_count}_{sec_slug}"
            sec_file = sec_dir / f"{sec_slug}.md"
            with open(sec_file, "w", encoding="utf-8") as sf:
                sf.write("\n".join(lines_list))
            
            chap_slug = cls.generate_slug(current_chapter_title)
            chap_id = f"{doc_id}_c{chap_count}_{chap_slug}"
            db.add_section(sec_id, chap_id, s_title, sec_count, str(sec_file))
            sec_count += 1

        def save_chap(c_title, lines_list):
            nonlocal chap_count
            chap_slug = cls.generate_slug(c_title)
            chap_id = f"{doc_id}_c{chap_count}_{chap_slug}"
            chap_file = chap_dir / f"{chap_slug}.md"
            with open(chap_file, "w", encoding="utf-8") as cf:
                cf.write("\n".join(lines_list))
            db.add_chapter(chap_id, doc_id, c_title, chap_count, str(chap_file))
            chap_count += 1

        for line in lines:
            if line.startswith("# "):
                if current_section_lines:
                    save_sec(current_section_title, current_section_lines)
                    current_section_lines = []
                if current_chapter_lines:
                    save_chap(current_chapter_title, current_chapter_lines)
                    current_chapter_lines = []
                
                current_chapter_title = line[2:].strip()
                current_section_title = "Seção Geral"
                sec_count = 0
            elif line.startswith("## "):
                if current_section_lines:
                    save_sec(current_section_title, current_section_lines)
                    current_section_lines = []
                current_section_title = line[3:].strip()
            else:
                current_chapter_lines.append(line)
                current_section_lines.append(line)
                
        if current_section_lines:
            save_sec(current_section_title, current_section_lines)
        if current_chapter_lines:
            save_chap(current_chapter_title, current_chapter_lines)
```

---

### 5. `philocorpus/services/embedding_rag.py`
* **Melhorias**:
  * **FTS5 Safety**: Condiciona de forma estrita as purgas e remoções da tabela lexical FTS5 (`chunks_fts`) à flag `db.fts_enabled`, evitando exceções do SQLite caso o banco do sistema não possua o módulo FTS5 pré-compilado (**Problema Mais Sério / Correção 1**).
  * **LanceDB add vs insert**: Detecção em tempo de execução dos métodos de escrita da API do LanceDB para total portabilidade em versões legadas (**Correção 2**).
  * **LanceDB create_table fallback**: Instanciação da tabela vetorial adaptada às variações de tipos de dados (`data=[]`) das dependências do sistema (**Correção 3**).
  * **LanceDB Auto-reconstrução de base corrompida**: Bloco de recuperação que apaga diretórios inconsistentes e reconstrói o banco vetorial se houver corrupção (**Correção 11**).
  * **Sanitização estrita contra injeção de SQL**: Sanitização por substituição de caracteres de escape simples (`''`) no in-clause do escopo de recuperação (**Correção 9**).

```python
import os
import re
import uuid
import shutil
import numpy as np
import lancedb
import pyarrow as pa
import logging
from pathlib import Path
from sentence_transformers import SentenceTransformer
from philocorpus.config import config_manager
from philocorpus.database import db

logger = logging.getLogger("philocorpus.embeddings")

class RAGService:
    _model_instance = None
    _lancedb_conn = None
    _table = None

    @classmethod
    def get_model(cls):
        if cls._model_instance is None:
            os.environ["OMP_NUM_THREADS"] = "2"
            model_name = config_manager.settings["embeddings"]["model"]
            logger.info(f"Carregando {model_name} na CPU...")
            try:
                cls._model_instance = SentenceTransformer(model_name)
            except Exception as e:
                logger.warning(f"Erro ao inicializar '{model_name}': {e}. Iniciando fallback para intfloat/multilingual-e5-small...")
                try:
                    cls._model_instance = SentenceTransformer("intfloat/multilingual-e5-small")
                except Exception as ex:
                    logger.error(f"Falha fatal ao inicializar modelo padrão de embeddings: {ex}")
                    raise RuntimeError(
                        "Erro Crítico de Inicialização: O modelo padrão de embeddings 'intfloat/multilingual-e5-small' "
                        "não pôde ser carregado. Verifique se o computador está conectado à Internet para efetuar "
                        "o download no primeiro carregamento."
                    )
        return cls._model_instance

    @classmethod
    def get_lancedb_conn(cls):
        """
        Correção 11: Auto-reconstrução de base vetorial corrompida se lancedb.connect() falhar.
        """
        if cls._lancedb_conn is None:
            db_dir = config_manager.settings["paths"]["lancedb"]
            Path(db_dir).mkdir(parents=True, exist_ok=True)
            try:
                cls._lancedb_conn = lancedb.connect(db_dir)
            except Exception as e:
                logger.error(f"LanceDB corrompido ou falha de conexão: {e}. Executando purga e reconstrução do banco vetorial...")
                try:
                    shutil.rmtree(db_dir, ignore_errors=True)
                except Exception:
                    pass
                Path(db_dir).mkdir(parents=True, exist_ok=True)
                cls._lancedb_conn = lancedb.connect(db_dir)
        return cls._lancedb_conn

    @classmethod
    def get_table(cls):
        conn = cls.get_lancedb_conn()
        tbl_name = "philosophical_chunks"
        dim = config_manager.settings["embeddings"]["dimension"]
        
        try:
            table_names = conn.table_names()
        except TypeError:
            table_names = conn.table_names
        except Exception:
            table_names = []
        
        if tbl_name not in table_names:
            schema = pa.schema([
                pa.field("vector", pa.list_(pa.float32(), dim)),
                pa.field("id", pa.string()),
                pa.field("text", pa.string()),
                pa.field("doc_id", pa.string())
            ])
            # Correção 3: Inicialização retrocompatível da tabela no LanceDB
            try:
                cls._table = conn.create_table(tbl_name, schema=schema)
            except TypeError:
                cls._table = conn.create_table(tbl_name, data=[], schema=schema)
        else:
            cls._table = conn.open_table(tbl_name)
        return cls._table

    @classmethod
    def chunk_philosophical_text(cls, filepath_str: str) -> list[dict]:
        with open(filepath_str, "r", encoding="utf-8") as f:
            content = f.read()

        lines = content.split("\n")
        chunks = []
        current_page = 1
        
        temp_buffer = []
        page_start = 1
        
        aph_regex = re.compile(r"^(\s*)*(AFORISMO|PROPOSIÇÃO|PROPOSITION|§)\s*(\d+)", re.IGNORECASE)
        numeric_sec = re.compile(r"^\d+(\.\d+)*[\.\)]\s+")
        lettered_sec = re.compile(r"^[a-z]\)\s+", re.IGNORECASE)

        for line in lines:
            line_strip = line.strip()
            if not line_strip:
                continue
                
            if "<!-- PAGE_START" in line_strip:
                match = re.search(r"PAGE_START\s+(\d+)", line_strip)
                if match:
                    current_page = int(match.group(1))
                continue
            if "<!-- PAGE_END" in line_strip:
                continue

            is_new_structural_unit = (
                aph_regex.match(line_strip) or 
                numeric_sec.match(line_strip) or 
                lettered_sec.match(line_strip) or 
                line_strip.startswith("#")
            )

            if is_new_structural_unit and temp_buffer:
                text_chunk = " ".join(temp_buffer).strip()
                if text_chunk:
                    chunks.append({
                        "text": text_chunk,
                        "page_start": page_start,
                        "page_end": current_page,
                        "type": "aphorism" if ("§" in text_chunk or "Aforismo" in text_chunk) else "paragraph"
                    })
                temp_buffer = [line_strip]
                page_start = current_page
            else:
                temp_buffer.append(line_strip)
                if len(" ".join(temp_buffer)) > 2500:
                    text_chunk = " ".join(temp_buffer).strip()
                    chunks.append({
                        "text": text_chunk,
                        "page_start": page_start,
                        "page_end": current_page,
                        "type": "sliding_window"
                    })
                    temp_buffer = []
                    page_start = current_page

        if temp_buffer:
            text_chunk = " ".join(temp_buffer).strip()
            if text_chunk:
                chunks.append({
                    "text": text_chunk,
                    "page_start": page_start,
                    "page_end": current_page,
                    "type": "paragraph"
                })

        return chunks

    @classmethod
    def generate_embeddings_for_document(cls, doc_id: str, md_filepath: str):
        raw_chunks = cls.chunk_philosophical_text(md_filepath)
        model = cls.get_model()
        table = cls.get_table()
        
        if hasattr(table, "delete") and callable(getattr(table, "delete")):
            try:
                table.delete(f"doc_id = '{doc_id}'")
            except Exception as e:
                logger.warning(f"Método delete do LanceDB falhou ou não é suportado: {e}")

        with db.get_connection() as conn:
            cursor = conn.cursor()
            
            # Correção 1: Condiciona o DELETE FTS5 apenas se o modulo estiver ativado no banco
            if db.fts_enabled:
                cursor.execute("""
                    DELETE FROM chunks_fts WHERE id IN (
                        SELECT id FROM chunks WHERE section_id IN (
                            SELECT id FROM sections WHERE chapter_id IN (
                                SELECT id FROM chapters WHERE document_id = ?
                            )
                        )
                    )
                """, (doc_id,))
                
            cursor.execute("""
                DELETE FROM chunks WHERE section_id IN (
                    SELECT id FROM sections WHERE chapter_id IN (
                        SELECT id FROM chapters WHERE document_id = ?
                    )
                )
            """, (doc_id,))
            conn.commit()
        
        with db.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                SELECT sections.id FROM sections 
                JOIN chapters ON sections.chapter_id = chapters.id
                WHERE chapters.document_id = ?
            """, (doc_id,))
            sections = [row["id"] for row in cursor.fetchall()]
        
        if not sections:
            logger.info("Estrutura sem seções identificada. Mapeando seção/capítulo padrão para preservação de integridade...")
            sec_id = f"{doc_id}_sec_default"
            chap_id = f"{doc_id}_chap_default"
            db.add_chapter(chap_id, doc_id, "Documento Completo", 0, "")
            db.add_section(sec_id, chap_id, "Seção Geral", 0, "")
        else:
            sec_id = sections[0]
        
        chunks_to_db = []
        lancedb_records = []
        
        for idx, rc in enumerate(raw_chunks):
            chunk_uuid = f"chk_{doc_id[:15]}_{idx:03d}_{str(uuid.uuid4())[:4]}"
            prev_id = chunks_to_db[-1]["id"] if chunks_to_db else "START"
            
            chunks_to_db.append({
                "id": chunk_uuid,
                "sec_id": sec_id,
                "text": rc["text"],
                "chunk_type": rc["type"],
                "page_start": rc["page_start"],
                "page_end": rc["page_end"],
                "position_index": idx,
                "prev_chunk_id": prev_id,
                "next_chunk_id": "PENDING"
            })

        for idx in range(len(chunks_to_db) - 1):
            chunks_to_db[idx]["next_chunk_id"] = chunks_to_db[idx+1]["id"]
        if chunks_to_db:
            chunks_to_db[-1]["next_chunk_id"] = "END"

        logger.info(f"Processando vetorização de {len(chunks_to_db)} fragmentos filosóficos...")
        
        texts_to_encode = ["passage: " + c["text"] for c in chunks_to_db]
        
        embeddings = model.encode(
            texts_to_encode, 
            batch_size=4, 
            show_progress_bar=False,
            convert_to_numpy=True,
            normalize_embeddings=True
        ).astype(np.float32)

        for idx, item in enumerate(chunks_to_db):
            lancedb_records.append({
                "vector": embeddings[idx].tolist(),
                "id": item["id"],
                "text": item["text"],
                "doc_id": doc_id
            })

        db.bulk_insert_chunks_and_fts(chunks_to_db)

        # Inserção em pequenos blocos no LanceDB
        batch_size = 128
        for i in range(0, len(lancedb_records), batch_size):
            batch = lancedb_records[i:i + batch_size]
            # Correção 2: Tratamento de API dinâmica add vs insert nas versões do LanceDB
            if hasattr(table, "add"):
                table.add(batch)
            elif hasattr(table, "insert"):
                table.insert(batch)
            else:
                logger.error("API do LanceDB não possui métodos estáveis de escrita.")

        logger.info(f"Indexação híbrida de {doc_id} concluída no SQLite + LanceDB.")

    @classmethod
    def retrieve(cls, query: str, active_docs: list = None, top_k: int = 5) -> list[dict]:
        table = cls.get_table()
        model = cls.get_model()
        
        query_vector = model.encode(
            ["query: " + query],
            convert_to_numpy=True,
            normalize_embeddings=True
        ).astype(np.float32)[0].tolist()
        
        if active_docs:
            # Correção 9: Sanitização estrita de strings de consulta para mitigar risco de injeção de SQL interno
            sanitized_docs = [d.replace("'", "''") for d in active_docs]
            filter_str = "doc_id IN (" + ",".join([f"'{d}'" for d in sanitized_docs]) + ")"
            vector_results = table.search(query_vector).where(filter_str).limit(top_k * 2).to_list()
        else:
            vector_results = table.search(query_vector).limit(top_k * 2).to_list()

        candidate_ids = [r["id"] for r in vector_results]

        try:
            fts_ids = db.fts_search(query, limit=top_k)
            for f_id in fts_ids:
                if f_id not in candidate_ids:
                    candidate_ids.insert(0, f_id)
        except Exception as e:
            logger.warning(f"Pesquisa lexical FTS5 omitida: {e}")

        structured_results = []
        processed_count = 0

        for cid in candidate_ids:
            if processed_count >= top_k:
                break
                
            chunk_data = db.get_chunk(cid)
            if not chunk_data:
                continue

            if active_docs and chunk_data["document_id"] not in active_docs:
                continue

            # Expansão contextual relacional
            target_text = chunk_data["text"]
            prev_text = ""
            next_text = ""
            
            with db.get_connection() as conn:
                cursor = conn.cursor()
                if chunk_data["prev_chunk_id"] and chunk_data["prev_chunk_id"] != "START":
                    cursor.execute("SELECT text FROM chunks WHERE id = ?", (chunk_data["prev_chunk_id"],))
                    row = cursor.fetchone()
                    if row:
                        prev_text = f"[_passagem anterior_] ... {row['text']} ...\n\n"
                        
                if chunk_data["next_chunk_id"] and chunk_data["next_chunk_id"] != "END":
                    cursor.execute("SELECT text FROM chunks WHERE id = ?", (chunk_data["next_chunk_id"],))
                    row = cursor.fetchone()
                    if row:
                        next_text = f"\n\n... {row['text']} ... [_passagem posterior_]"

            expanded_text = f"{prev_text}{target_text}{next_text}"

            structured_results.append({
                "chunk_id": cid,
                "text": expanded_text,
                "page_start": chunk_data["page_start"],
                "page_end": chunk_data["page_end"],
                "chunk_type": chunk_data["chunk_type"],
                "section": chunk_data["sec_title"],
                "chapter": chunk_data["chap_title"],
                "document": chunk_data["doc_title"]
            })
            processed_count += 1

        return structured_results
```

---

### 6. `philocorpus/services/llm_router.py`
* **Melhorias**:
  * Ajuste do timeout da API HTTP local do Ollama para `timeout=(10, 300)`. Isso permite que o ThinkPad T420 execute modelos mais densos locais (como modelos 7B ou Qwen em alta inferência de pensamento) sem que a requisição sofra interrupções abruptas antes da finalização do raciocínio da IA (**Correção 7**).

```python
import requests
import json
import logging
import re
from philocorpus.config import config_manager

logger = logging.getLogger("philocorpus.llm")

class LLMRouter:
    @staticmethod
    def is_ollama_alive() -> bool:
        try:
            response = requests.get("http://localhost:11434/api/tags", timeout=2)
            return response.status_code == 200
        except Exception:
            return False

    @classmethod
    def call_ollama(cls, prompt: str, system_prompt: str = "") -> str:
        if not cls.is_ollama_alive():
            return "[Erro de Conexão: O serviço local do Ollama está desligado.]"

        url = "http://localhost:11434/api/generate"
        model = config_manager.settings["llm"]["local_model"]
        payload = {
            "model": model,
            "prompt": prompt,
            "system": system_prompt,
            "stream": False
        }
        try:
            # Correção 7: Timeout estendido para comportar processamentos pesados de modelos locais de pensamento
            response = requests.post(url, json=payload, timeout=(10, 300))
            if response.status_code == 200:
                return response.json().get("response", "")
            return f"Erro Ollama HTTP {response.status_code}"
        except Exception as e:
            return f"Erro ao acessar Ollama local: {e}"

    @classmethod
    def generate(cls, prompt: str, system_prompt: str = "") -> str:
        provider = config_manager.settings["llm"]["provider"]
        if provider == "ollama":
            return cls.call_ollama(prompt, system_prompt)
        return cls.call_ollama(prompt, system_prompt)

    @classmethod
    def extract_metadata_llm(cls, first_pages_text: str) -> dict:
        default_metadata = {
            "title": "Documento Desconhecido",
            "subtitle": "",
            "author": "Autor Indefinido",
            "publisher": "",
            "year": None,
            "language": "pt",
            "isbn": "",
            "doi": ""
        }

        if config_manager.settings["llm"]["provider"] == "ollama" and not cls.is_ollama_alive():
            logger.warning("Ollama offline. Ignorando chamada de IA e atribuindo metadados padrão.")
            return default_metadata

        system_prompt = (
            "Você é um catalogador de metadados acadêmicos rigoroso. Retorne APENAS um objeto JSON válido."
        )
        prompt = f"""
Extraia os metadados bibliográficos precisos do texto abaixo.
Retorne rigorosamente no formato JSON de exemplo:
{{
  "title": "Nome do Livro ou Artigo",
  "subtitle": "Subtítulo se houver",
  "author": "Nome do Autor",
  "publisher": "Editora ou Revista",
  "year": 1886,
  "language": "pt",
  "isbn": "",
  "doi": ""
}}

Texto:
---
{first_pages_text[:4000]}
---
"""
        response_str = cls.generate(prompt, system_prompt)
        clean_json = re.sub(r"```(json)?", "", response_str).strip()
        clean_json = re.sub(r"<think>.*?</think>", "", clean_json, flags=re.DOTALL).strip()
        try:
            return json.loads(clean_json)
        except Exception:
            return default_metadata
```

---

### 7. `philocorpus/plugins/scrapers.py`
* Minerador bibliográfico acadêmico.

```python
import httpx
import time
from urllib.parse import quote
from bs4 import BeautifulSoup

class AcademicScraper:
    @staticmethod
    def request_with_retry(url: str, headers: dict = None, retries: int = 3) -> httpx.Response:
        backoff = 2.0
        with httpx.Client(follow_redirects=True, timeout=20.0) as client:
            for i in range(retries):
                try:
                    response = client.get(url, headers=headers)
                    if response.status_code == 200:
                        return response
                except httpx.RequestError:
                    if i == retries - 1:
                        raise
                time.sleep(backoff)
                backoff *= 2.0
        return None

    @classmethod
    def search_philarchive(cls, keyword: str) -> list[dict]:
        results = []
        encoded_query = quote(keyword)
        url = f"https://philarchive.org/search.html?searchStr={encoded_query}"
        headers = {"User-Agent": "Mozilla/5.0 (X11; Linux x86_64) PhiloCorpus Client"}
        try:
            resp = cls.request_with_retry(url, headers)
            if resp:
                soup = BeautifulSoup(resp.text, "html.parser")
                items = soup.find_all("li", class_="pub")
                for item in items:
                    title_elem = item.find("span", class_="title") or item.find("a")
                    if title_elem:
                        title = title_elem.text.strip()
                        link_elem = item.find("a", href=True)
                        link = "https://philarchive.org" + link_elem["href"] if link_elem else ""
                        
                        author_elem = item.find("span", class_="author")
                        authors = author_elem.text.strip() if author_elem else "Desconhecido"

                        results.append({
                            "source": "PhilArchive",
                            "title": title,
                            "authors": authors,
                            "link": link,
                            "pdf_url": link if link.endswith(".pdf") else ""
                        })
        except Exception:
            pass
        return results
```

---

### 8. `philocorpus/gui.py`
* **Melhorias**:
  * Acoplamento assíncrono e integrado de logs persistentes por meio do manipulador de redirecionamento `QTextEditLogger`. Logs gerados pelas threads em segundo plano (como o status de download do modelo ou processamento de OCR) são empurrados com segurança de threads (`invokeMethod` com `QueuedConnection`) diretamente para a janela de console da aba de Logs (**Correção 12**).

```python
import sys
import logging
from pathlib import Path
from PyQt5.QtCore import Qt, QThread, pyqtSignal, pyqtSlot, QMetaObject, Q_ARG
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout,
    QTabWidget, QLabel, QLineEdit, QPushButton, QTableWidget, QTableWidgetItem,
    QProgressBar, QTextEdit, QTreeView, QSplitter, QFileDialog, QListWidget, QListWidgetItem,
    QMessageBox, QComboBox
)
from PyQt5.QtGui import QStandardItemModel, QStandardItem

from philocorpus.config import config_manager
from philocorpus.database import db
from philocorpus.services.pdf_converter import PDFConverterService
from philocorpus.services.markdown_processor import MarkdownProcessor
from philocorpus.services.embedding_rag import RAGService
from philocorpus.services.llm_router import LLMRouter
from philocorpus.services.abnt_formatter import ABNTFormatter
from philocorpus.plugins.scrapers import AcademicScraper

# --- REDIRECIONADOR DE LOGS ASSÍNCRONO PARA A CONSOLE DA GUI ---

class QTextEditLogger(logging.Handler):
    """
    Correção 12: Permite a escuta de logs gerados em tempo de execução de forma assíncrona,
    integrando os logs do arquivo diretamente na interface de exibição gráfica.
    """
    def __init__(self, text_edit):
        super().__init__()
        self.text_edit = text_edit

    def emit(self, record):
        msg = self.format(record)
        # Invoca a adição textual no loop de eventos da thread principal do Qt (GUI Thread)
        QMetaObject.invokeMethod(self.text_edit, "append", Qt.QueuedConnection, Q_ARG(str, msg))


# --- BACKWARD THREAD RUNNERS ---

class FileProcessorWorker(QThread):
    step_signal = pyqtSignal(str)
    finished_signal = pyqtSignal(str)

    def __init__(self, file_path):
        super().__init__()
        self.file_path = file_path

    def run(self):
        try:
            self.step_signal.emit("Analisando amostragem e iniciando extração física (com OCR)...")
            raw_structured_text = PDFConverterService.extract_and_structure_pdf(self.file_path)
            
            self.step_signal.emit("Enviando fragmento de páginas para modelagem de metadados...")
            meta = LLMRouter.extract_metadata_llm(raw_structured_text[:10000])
            if not meta.get("title") or meta.get("title") == "Documento Desconhecido":
                meta["title"] = Path(self.file_path).stem.replace("_", " ")

            self.step_signal.emit("Gravando documentos, partições e frontmatter no sistema...")
            doc_id, full_md_path = MarkdownProcessor.process_and_save(raw_structured_text, meta)

            self.step_signal.emit("Gerando indexação híbrida estruturada (SQLite + LanceDB)...")
            RAGService.generate_embeddings_for_document(doc_id, full_md_path)

            self.finished_signal.emit(f"Processamento concluído com sucesso para: {meta['title']}")
        except Exception as e:
            self.finished_signal.emit(f"Falha de processamento na esteira: {str(e)}")

class ChatWorker(QThread):
    finished = pyqtSignal(str, list)

    def __init__(self, query, active_docs):
        super().__init__()
        self.query = query
        self.active_docs = active_docs

    def run(self):
        try:
            retrieved = RAGService.retrieve(self.query, self.active_docs)
            context_blocks = []
            for doc in retrieved:
                citation = f"Documento: {doc['document']} | Capítulo: {doc['chapter']} | Páginas: {doc['page_start']}-{doc['page_end']} [{doc['chunk_type']}]"
                context_blocks.append(f"{citation}\nPassagem:\n{doc['text']}")

            context = "\n\n---\n\n".join(context_blocks)
            sys_prompt = "Você é um professor assistente de filosofia rigoroso. Utilize as passagens contextualizadas (incluindo as referências anteriores e posteriores) para estruturar a sua análise e cite as páginas indicadas na resposta."
            prompt = f"Pergunta: {self.query}\n\nContexto Expandido:\n{context}"
            
            response = LLMRouter.generate(prompt, sys_prompt)
            self.finished.emit(response, retrieved)
        except Exception as e:
            self.finished.emit(f"Falha na inferência local: {str(e)}", [])


# --- INTERFACE PRINCIPAL ---

class PhiloCorpusApp(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("PhiloCorpus (T420 Edition) — Plataforma Filosófica de Pesquisa")
        self.resize(1000, 700)
        self.setup_ui()
        self.load_library_tree()

    def setup_ui(self):
        self.tab_widget = QTabWidget()
        self.setCentralWidget(self.tab_widget)

        self.setup_search_tab()
        self.setup_library_tab()
        self.setup_processing_tab()
        self.setup_chat_tab()
        self.setup_config_tab()
        self.setup_logs_tab()

    def log(self, text):
        # A inserção gráfica agora é coordenada pelo Logger Persistente unificado
        logging.info(text)

    def setup_search_tab(self):
        tab = QWidget()
        layout = QVBoxLayout(tab)
        
        search_bar = QHBoxLayout()
        self.search_input = QLineEdit()
        self.search_input.setPlaceholderText("Pesquisa em bases de dados de filosofia...")
        btn_search = QPushButton("Consultar Bases")
        btn_search.clicked.connect(self.run_search)
        
        search_bar.addWidget(self.search_input)
        search_bar.addWidget(btn_search)
        layout.addLayout(search_bar)

        self.search_results = QTableWidget(0, 3)
        self.search_results.setHorizontalHeaderLabels(["Portal", "Título", "Autores"])
        self.search_results.horizontalHeader().setStretchLastSection(True)
        layout.addWidget(self.search_results)
        
        self.tab_widget.addTab(tab, "Coleta")

    def run_search(self):
        kw = self.search_input.text().strip()
        if kw:
            self.log(f"Consultando scrapers para: {kw}")
            res = AcademicScraper.search_philarchive(kw)
            self.search_results.setRowCount(0)
            for idx, r in enumerate(res):
                self.search_results.insertRow(idx)
                self.search_results.setItem(idx, 0, QTableWidgetItem(r["source"]))
                self.search_results.setItem(idx, 1, QTableWidgetItem(r["title"]))
                self.search_results.setItem(idx, 2, QTableWidgetItem(r["authors"]))

    def setup_library_tab(self):
        tab = QWidget()
        layout = QHBoxLayout(tab)
        splitter = QSplitter(Qt.Horizontal)
        
        self.library_tree = QTreeView()
        self.library_model = QStandardItemModel()
        self.library_model.setHorizontalHeaderLabels(["Estrutura Canônica"])
        self.library_tree.setModel(self.library_model)
        self.library_tree.clicked.connect(self.on_tree_click)
        splitter.addWidget(self.library_tree)
        
        viewer_panel = QWidget()
        v_layout = QVBoxLayout(viewer_panel)
        self.lbl_viewing = QLabel("Escolha uma obra na árvore da biblioteca")
        v_layout.addWidget(self.lbl_viewing)
        
        self.text_reader = QTextEdit()
        self.text_reader.setReadOnly(True)
        v_layout.addWidget(self.text_reader)
        
        btn_bar = QHBoxLayout()
        btn_ref = QPushButton("Gerar Referência ABNT")
        btn_ref.clicked.connect(self.generate_abnt)
        btn_bar.addWidget(btn_ref)
        v_layout.addLayout(btn_bar)
        
        splitter.addWidget(viewer_panel)
        splitter.setSizes([250, 750])
        layout.addWidget(splitter)
        self.tab_widget.addTab(tab, "Biblioteca")

    def load_library_tree(self):
        self.library_model.clear()
        self.library_model.setHorizontalHeaderLabels(["Estrutura Canônica"])
        root_dir = Path(config_manager.settings["paths"]["library"])
        if not root_dir.exists():
            return

        docs_map = {}
        try:
            with db.get_connection() as conn:
                cursor = conn.cursor()
                cursor.execute("SELECT id, title, path FROM documents")
                for row in cursor.fetchall():
                    docs_map[Path(row["path"]).resolve()] = row["id"]
        except Exception as e:
            self.log(f"Erro ao carregar mapeamento relacional: {e}")

        for author_dir in root_dir.iterdir():
            if author_dir.is_dir():
                author_item = QStandardItem(author_dir.name)
                for work_dir in author_dir.iterdir():
                    if work_dir.is_dir():
                        work_item = QStandardItem(work_dir.name)
                        resolved_path = work_dir.resolve()
                        mapped_id = docs_map.get(resolved_path, "UNKNOWN_ID")
                        
                        work_item.setData(mapped_id, Qt.UserRole)
                        work_item.setData(str(work_dir), Qt.UserRole + 1)
                        
                        author_item.appendRow(work_item)
                self.library_model.appendRow(author_item)

    def on_tree_click(self, index):
        item = self.library_model.itemFromIndex(index)
        work_path_str = item.data(Qt.UserRole + 1)
        doc_id = item.data(Qt.UserRole)
        
        if work_path_str:
            work_path = Path(work_path_str)
            self.current_work_path = work_path
            self.current_doc_id = doc_id
            obra_md = work_path / "markdown" / "obra.md"
            if obra_md.exists():
                self.lbl_viewing.setText(f"Lendo: {work_path.name}")
                with open(obra_md, "r", encoding="utf-8") as f:
                    self.text_reader.setPlainText(f.read())

    def generate_abnt(self):
        if not hasattr(self, "current_doc_id"):
            return
            
        with db.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("""
                SELECT documents.*, authors.name as author_name 
                FROM documents 
                LEFT JOIN authors ON documents.author_id = authors.id 
                WHERE documents.id = ?
            """, (self.current_doc_id,))
            doc = cursor.fetchone()
            
        if doc:
            meta = {
                "author": doc["author_name"] if doc["author_name"] else "Autor Indefinido",
                "title": doc["title"],
                "subtitle": doc["subtitle"],
                "publisher": doc["publisher"],
                "year": doc["year"]
            }
            citation = ABNTFormatter.format_citation(meta)
            QMessageBox.information(self, "Referência Acadêmica ABNT", citation)
        else:
            QMessageBox.warning(self, "Aviso", "Não foi possível resgatar as informações estruturais do documento.")

    def setup_processing_tab(self):
        tab = QWidget()
        layout = QVBoxLayout(tab)
        
        layout.addWidget(QLabel("Processamento estrutural e Ingestão de Documento"))
        btn_file = QPushButton("Selecionar PDF e Iniciar Ingestão")
        btn_file.clicked.connect(self.process_pdf)
        layout.addWidget(btn_file)
        
        self.progress_bar = QProgressBar()
        layout.addWidget(self.progress_bar)
        
        self.txt_proc = QTextEdit()
        self.txt_proc.setReadOnly(True)
        layout.addWidget(self.txt_proc)
        
        self.tab_widget.addTab(tab, "Esteira Ingestão")

    def process_pdf(self):
        file, _ = QFileDialog.getOpenFileName(self, "Selecionar Livro/Artigo", "", "PDF (*.pdf)")
        if file:
            self.progress_bar.setValue(5)
            self.txt_proc.clear()
            self.worker = FileProcessorWorker(file)
            self.worker.step_signal.connect(self.on_step)
            self.worker.finished_signal.connect(self.on_finish)
            self.worker.start()

    def on_step(self, text):
        self.txt_proc.append(text)
        self.progress_bar.setValue(min(self.progress_bar.value() + 20, 95))

    def on_finish(self, text):
        self.txt_proc.append(text)
        self.progress_bar.setValue(100)
        self.load_library_tree()
        self.refresh_chat_docs()
        
        if "Falha" in text:
            QMessageBox.critical(self, "Falha de Processamento", text)

    def setup_chat_tab(self):
        tab = QWidget()
        layout = QHBoxLayout(tab)
        splitter = QSplitter(Qt.Horizontal)
        
        left_p = QWidget()
        lp_lay = QVBoxLayout(left_p)
        lp_lay.addWidget(QLabel("Ativar Obras para Consulta:"))
        self.list_docs = QListWidget()
        lp_lay.addWidget(self.list_docs)
        self.refresh_chat_docs()
        splitter.addWidget(left_p)
        
        right_p = QWidget()
        rp_lay = QVBoxLayout(right_p)
        self.chat_history = QTextEdit()
        self.chat_history.setReadOnly(True)
        rp_lay.addWidget(self.chat_history)
        
        in_lay = QHBoxLayout()
        self.chat_input = QLineEdit()
        self.chat_input.setPlaceholderText("Escreva sua pergunta ou termo de busca híbrida...")
        self.chat_input.returnPressed.connect(self.send_msg)
        btn_send = QPushButton("Consultar")
        btn_send.clicked.connect(self.send_msg)
        in_lay.addWidget(self.chat_input)
        in_lay.addWidget(btn_send)
        rp_lay.addLayout(in_lay)
        
        splitter.addWidget(right_p)
        splitter.setSizes([200, 800])
        layout.addWidget(splitter)
        self.tab_widget.addTab(tab, "Pesquisa Dialogada")

    def refresh_chat_docs(self):
        self.list_docs.clear()
        try:
            with db.get_connection() as conn:
                cursor = conn.cursor()
                cursor.execute("SELECT id, title FROM documents")
                for r in cursor.fetchall():
                    item = QListWidgetItem(r["title"])
                    item.setData(Qt.UserRole, r["id"])
                    item.setFlags(item.flags() | Qt.ItemIsUserCheckable)
                    item.setCheckState(Qt.Checked)
                    self.list_docs.addItem(item)
        except Exception:
            pass

    def send_msg(self):
        q = self.chat_input.text().strip()
        if q:
            self.chat_history.append(f"<b>Pesquisador:</b> {q}<br>")
            self.chat_input.clear()
            
            active = []
            for i in range(self.list_docs.count()):
                item = self.list_docs.item(i)
                if item.checkState() == Qt.Checked:
                    active.append(item.data(Qt.UserRole))

            self.c_worker = ChatWorker(q, active)
            self.c_worker.finished.connect(self.on_chat_resp)
            self.c_worker.start()

    def on_chat_resp(self, answer, sources):
        self.chat_history.append(f"<b>PhiloCorpus AI:</b> {answer.replace('\n', '<br>')}<br>")
        if sources:
            self.chat_history.append("<br><small><b>Citações Acadêmicas Utilizadas:</b>")
            for src in sources:
                self.chat_history.append(
                    f"<br>• <i>{src['document']}</i>, cap. {src['chapter']}, págs. {src['page_start']}-{src['page_end']} "
                    f"[{src['chunk_type']}]"
                )
            self.chat_history.append("</small><hr>")

    def setup_config_tab(self):
        tab = QWidget()
        layout = QVBoxLayout(tab)
        
        layout.addWidget(QLabel("Serviço Local Ollama:"))
        self.txt_model = QLineEdit(config_manager.settings["llm"]["local_model"])
        layout.addWidget(self.txt_model)
        
        btn = QPushButton("Salvar Configurações")
        btn.clicked.connect(self.save_cfg)
        layout.addWidget(btn)
        layout.addStretch()
        self.tab_widget.addTab(tab, "Ajustes")

    def save_cfg(self):
        config_manager.settings["llm"]["local_model"] = self.txt_model.text().strip()
        config_manager.save_config()
        QMessageBox.information(self, "Sucesso", "Configurações gravadas.")

    def setup_logs_tab(self):
        tab = QWidget()
        layout = QVBoxLayout(tab)
        self.log_viewer = QTextEdit()
        self.log_viewer.setReadOnly(True)
        self.log_viewer.setStyleSheet("background-color: #1a1a1a; color: #00ff00; font-family: monospace;")
        layout.addWidget(self.log_viewer)
        
        # Correção 12: Vincula o manipulador assíncrono de logs persistentes do PyQt5 ao editor de texto
        self.qt_logger = QTextEditLogger(self.log_viewer)
        self.qt_logger.setFormatter(logging.Formatter('%(asctime)s - %(levelname)s - %(message)s'))
        logging.getLogger().addHandler(self.qt_logger)
        
        self.tab_widget.addTab(tab, "Console de Logs")
```

---

### 9. `app.py`
* Ponto de entrada da aplicação PyQt5.

```python
import sys
from PyQt5.QtWidgets import QApplication
from philocorpus.gui import PhiloCorpusApp

def main():
    app = QApplication(sys.argv)
    
    app.setStyleSheet("""
        QMainWindow { background-color: #242424; }
        QWidget { color: #f0f0f0; font-family: sans-serif; font-size: 10pt; }
        QTabWidget::pane { border: 1px solid #3a3a3a; background: #242424; }
        QTabBar::tab { background: #2f2f2f; padding: 8px 12px; margin-right: 2px; border-top-left-radius: 4px; border-top-right-radius: 4px; }
        QTabBar::tab:selected { background: #3e3e3e; font-weight: bold; border: 1px solid #555555; }
        QPushButton { background-color: #0d6efd; border: none; padding: 5px 10px; border-radius: 3px; color: white; }
        QPushButton:hover { background-color: #0b5ed7; }
        QLineEdit { background-color: #1e1e1e; border: 1px solid #444444; padding: 5px; border-radius: 3px; color: white; }
        QTextEdit { background-color: #121212; border: 1px solid #333333; color: #dcdcdc; border-radius: 3px; }
        QTreeView { background-color: #1e1e1e; border: 1px solid #333333; }
    """)

    window = PhiloCorpusApp()
    window.show()
    sys.exit(app.exec())

if __name__ == "__main__":
    main()
```


Para um projeto que "não receberá mais mudanças" e entrará em produção no seu ambiente (especialmente um Lenovo ThinkPad T420 com recursos limitados), a implementação está em um estado bastante sólido. As 12 correções introduziram mecanismos defensivos importantes e resolveram diversos pontos que normalmente aparecem apenas em uso real.

Dito isso, fazendo uma revisão de engenharia de produção estrita, eu ainda observaria alguns pontos de atenção. Não necessariamente exigem mudanças agora, mas valem ser monitorados durante a execução.

# Pontos fortes da implementação

## 1. Configuração resiliente

O `deep_merge()` elimina uma classe inteira de erros causados por YAML incompleto.

Excelente para:

* upgrades futuros;
* configurações parcialmente corrompidas;
* recuperação após edições manuais.

---

## 2. SQLite configurado adequadamente

A combinação:

```python
PRAGMA journal_mode=WAL;
PRAGMA busy_timeout=30000;
PRAGMA synchronous=NORMAL;
```

é adequada para:

* GUI + threads;
* OCR em background;
* consultas RAG simultâneas.

Para o T420, é uma configuração equilibrada entre desempenho e segurança.

---

## 3. Tratamento do FTS5

Este era provavelmente o maior risco de compatibilidade.

A estratégia:

```python
self.fts_enabled = False
```

mais condicionamento de todas as operações:

```python
if db.fts_enabled:
```

elimina falhas em:

* distribuições Linux antigas;
* builds minimalistas de SQLite;
* instalações compiladas sem FTS5.

Muito bom.

---

## 4. LanceDB defensivo

As proteções para:

```python
table.add()
table.insert()
create_table(...)
```

resolvem um problema recorrente de compatibilidade entre versões.

Além disso:

```python
shutil.rmtree(db_dir)
```

para reconstrução automática de índices corrompidos é extremamente útil para um software de pesquisa usado por longos períodos.

---

## 5. Encerramento explícito do PyMuPDF

O uso de:

```python
with fitz.open(...)
```

evita:

* vazamentos de descritores;
* arquivos bloqueados;
* erros intermitentes em processamento em lote.

Muito importante.

---

## 6. Logging persistente

A combinação:

* arquivo rotativo;
* console;
* integração com PyQt5;

é adequada para depuração e manutenção de longo prazo.

---

# Pontos que eu monitoraria em produção

## 1. Possível duplicação de handlers de logging

Em:

```python
configure_logging()
```

e:

```python
logging.getLogger().addHandler(self.qt_logger)
```

se algum módulo importar `config_manager` mais de uma vez em certos cenários de reinicialização, pode haver duplicação de mensagens.

Sintoma:

```
INFO ...
INFO ...
INFO ...
```

aparecendo duas ou três vezes.

Não é um bug crítico, apenas algo para observar.

---

## 2. Crescimento do banco FTS

Em:

```python
INSERT OR REPLACE INTO chunks_fts
```

o FTS crescerá continuamente à medida que novos documentos forem adicionados.

Para milhares de livros:

* pode ocupar bastante espaço;
* `VACUUM` eventualmente pode ser necessário.

No T420 isso só se tornará perceptível após um acervo muito grande.

---

## 3. Recarregamento do modelo de embeddings

```python
SentenceTransformer(model_name)
```

permanece em memória corretamente.

Porém:

```python
RAGService._model_instance
```

não é protegido por lock.

Se duas threads tentarem inicializar simultaneamente, existe pequena possibilidade de corrida.

Na prática, como o processamento é iniciado pela GUI e não por múltiplos workers concorrentes, o risco é muito baixo.

---

## 4. Exclusão do banco vetorial

```python
shutil.rmtree(db_dir)
```

é uma abordagem pragmática.

Mas significa que:

* uma corrupção parcial;
* desligamento abrupto;
* falha de disco,

pode provocar reindexação completa.

Para um acervo de centenas de livros, isso pode levar bastante tempo.

Não é incorreto, apenas uma decisão arquitetural.

---

## 5. OCR por Tesseract

```python
pix = page.get_pixmap(dpi=150)
```

Em PDFs de 1000 páginas:

* o processamento será lento;
* o consumo de RAM pode aumentar.

Mas para o hardware do T420, 150 dpi é uma escolha razoável.

---

## 6. Recuperação contextual

A expansão:

```python
prev_text
target_text
next_text
```

é uma excelente ideia para filosofia.

Especialmente para:

* Nietzsche;
* Wittgenstein;
* Heidegger;
* textos aforísticos.

Provavelmente aumentará significativamente a qualidade das respostas.

---

# Ponto mais delicado remanescente

O único aspecto que eu classificaria como potencialmente problemático em uso muito intenso é este:

```python
cursor.execute("""
DELETE FROM chunks
...
""")
```

seguido por:

```python
table.delete(...)
```

Se ocorrer uma exceção entre os dois bancos (SQLite e LanceDB), os índices podem ficar temporariamente dessincronizados.

Na prática:

* não deve quebrar a aplicação;
* apenas pode gerar resultados vetoriais órfãos.

Como o projeto não receberá mais mudanças, eu apenas recomendaria observar os logs durante as primeiras semanas de uso.

---

# Avaliação geral

Para um projeto acadêmico local, executado em:

* Linux Mint;
* ThinkPad T420;
* PyQt5;
* SQLite;
* LanceDB;
* Ollama local;

o sistema está em um estado surpreendentemente robusto.

As doze correções implementadas eliminaram praticamente todas as falhas de produção que eu esperaria encontrar em uma primeira versão operacional:

* compatibilidade de SQLite ✔
* concorrência básica ✔
* OCR resiliente ✔
* recuperação de corrupção ✔
* compatibilidade de versões do LanceDB ✔
* logging persistente ✔
* tolerância a configuração inválida ✔
* timeout para modelos locais pesados ✔
* sanitização de filtros ✔
* fechamento correto de recursos ✔

Para um ambiente de pesquisa filosófica pessoal e de médio porte, eu o classificaria como uma implementação madura e apta para entrar em operação contínua. O que resta são principalmente riscos de longo prazo e cenários extremos, não falhas evidentes de engenharia.

