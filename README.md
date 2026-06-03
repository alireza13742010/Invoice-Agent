# Invoice-Agent
This workflow automates invoice processing end‑to‑end: it triggers on file updates in Google Drive, downloads the file, and routes it through a rules‑based Switch to either an internal AI Agent path (for text‑based PDFs and human review) or an external OCR path using Mistral (upload, signed URL if required, and retrieve OCR results). Both paths normalize their outputs and merge into a single stream that is enriched and validated by an OpenAI chat model to extract structured invoice fields (order ID, date, line items, totals, taxes). The final structured data is appended to a spreadsheet, the original file is archived to a processed folder, and the incoming file can be deleted to prevent reprocessing.

Key implementation notes: ensure binary handling is correct (downloaded files must appear under the exact binary key, e.g., `binary.data`, or use Move Binary Data to convert JSON→binary), enable **Send Binary Data** on HTTP upload nodes and do not manually set multipart `Content‑Type` headers so n8n can generate the boundary. Normalize schemas from both paths before merging, add validation logic to compare subtotal + taxes against the invoice total and route mismatches to manual review, and store API keys in n8n credentials rather than hardcoding. Include retry and error handling for network calls and consider webhook or polling strategies for asynchronous OCR providers.

# Demo 
<img width="1470" height="513" alt="image" src="https://github.com/user-attachments/assets/efb5bb6b-b6ac-4b06-8d7e-00afe531d13f" />

# Results
<img width="1073" height="193" alt="image" src="https://github.com/user-attachments/assets/df392842-3beb-4f85-b71f-fb84ae11ab49" />
