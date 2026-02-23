# ⚡ AI Invoice Intelligence: Multimodalny ETL
<a href="https://github.com/C8nraAD/-Doradca-Sk-adki-Ubezpieczeniowej-AI" target="_blank">Link do repozytorium </a>
##  O projekcie i wartości biznesowej
System klasy **Intelligent Document Processing (IDP)**, który automatyzuje proces ekstrakcji danych z faktur (PDF/IMG) i ich integrację z wewnętrznymi systemami firmy (SQL, CSV). Projekt eliminuje wąskie gardło w postaci manualnego przepisywania faktur, zapewniając automatyczną weryfikację cen z katalogiem produktów oraz walidację danych kontrahentów.

**Główne cele biznesowe:**
* **Automatyczna Audytowalność:** Wykrywanie rozbieżności między ceną na fakturze a ceną w oficjalnym cenniku (`ceny_sie_zgadzaja`).
* **Strukturyzacja Unstructured Data:** Zamiana nieustrukturyzowanych plików PDF/JPG na czysty format relacyjny.
* **Redukcja Błędów:** Wykorzystanie silnej typizacji (Pydantic) do wymuszenia poprawności danych już na etapie odczytu AI.

## 🛠 Technologie i Ekosystem
| Komponent | Technologia |
| :--- | :--- |
| **LLM Orchestration** | `instructor` (Strict Schema Enforcement) |
| **Core AI Model** | `OpenAI GPT-4o` (Multimodal) |
| **Data Engineering** | `Pandas` (Vectorized Joins & Merges) |
| **Validation** | `Pydantic v2` |
| **Database** | `SQLite` (Client Data), `CSV` (Price Lists) |
| **Image Processing** | `pdf2image`, `base64` encoding |

##  Kluczowe Rozwiązania Architektoniczne

Projekt realizuje pełny proces ETL (Extract, Transform, Load) z naciskiem na integralność danych:

### 1. Multimodalna Ekstrakcja 
Zamiast klasycznego OCR, system wykorzystuje model `gpt-4o` z wrapperem `instructor`. Pozwala to na:
* **Bezpośrednie mapowanie do obiektów Python:** Dane trafiają do klasy `InvoiceInfo`, co eliminuje błędy parsowania tekstu.
* **Obsługę PDF i Obrazów:** Automatyczna konwersja pierwszej strony PDF do formatu JPEG za pomocą `pdf2image`.

[Image of an OCR to SQL data pipeline architecture diagram showing image input, preprocessing, OCR engine, data validation, and SQL database storage]

### 2. Zaawansowana Logika Walidacji 
Zastosowanie schematów `InvoiceInfoItem` gwarantuje, że:
* Każda pozycja na fakturze posiada wymagane pola (`product_id`, `quantity`, `price`).
* Typy danych są zgodne (np. `date` jest obiektem typu datetime, a nie surowym stringiem).

### 3. Cross-Source Data Merging
Najsilniejszy punkt potoku — po ekstrakcji danych z AI, system wykonuje operacje na ramkach danych `Pandas`:
* **SQL Join:** Łączenie danych z faktury z bazą klientów (`zad_domowe__clients.db`).
* **CSV Lookup:** Porównywanie cen z faktury z katalogiem produktów (`zad_domowe__products.csv`).
* **Automatyczny Audit:** Wyliczanie kolumny `total_price` oraz flagowanie niezgodności cenowych.

##  Zarządzanie Stanem Plików 
Pipeline implementuje prostą maszynę stanów dla plików w systemie operacyjnym:
* **`raw/`**: Pliki oczekujące na procesowanie.
* **`processed/`**: Pliki pomyślnie przetworzone i dodane do raportu.
* **`error/`**: Dokumenty, których AI nie było w stanie zinterpretować (izolacja błędów).

## 📊 Efektywność Pipeline'u
* **Dokładność Ekstrakcji:** Dzięki GPT-4o i walidacji `instructor`, system radzi sobie z różnymi układami faktur bez konieczności definiowania reguł (Template-less OCR).
* **Integrity Check:** 100% poprawność sum kontrolnych dzięki weryfikacji krzyżowej z katalogiem produktów.
* **Output:** Jednolity raport `raport_koncowy.csv` gotowy do importu do systemów ERP/BI.

