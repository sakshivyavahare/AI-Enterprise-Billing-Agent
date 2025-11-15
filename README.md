# AI-Enterprise-Billing-Agent

<img width="1024" height="1024" alt="Gemini_Generated_Image_jyn9mejyn9mejyn9" src="https://github.com/user-attachments/assets/ea66c869-fb25-4e92-a741-79c7d6179afb" />


The AI Enterprise Billing Agent is a multi-agent automation system designed for modern businesses to streamline billing, GST calculations, transaction validation, and reporting. Many small and mid-size enterprises struggle with manual invoice processing, error-prone GST calculations (CGST/SGST/IGST), messy records, and monthly compliance preparation.
This project solves these challenges using a multi-agent architecture powered by LLM-based reasoning, custom tools, memory, and enterprise-grade observability.
The system reads invoices, extracts structured fields, validates amounts, auto-calculates GST, identifies errors, performs bulk Excel imports, and generates monthly reports — all with audit logs, metrics, and traceability.

This aligns with the Enterprise Agents track by improving business workflows, accuracy, decision-making, and automation.
1. Multi-Agent System
The system includes:
- Invoice Extraction Agent
Extracts invoice number, date, items, quantities, rates, taxable amount, and GST fields.
- GST Classification Agent
Determines tax type (CGST + SGST vs IGST) using rules based on buyer/seller states.
- GST Calculation Agent
Computes GST percentages and totals using a code execution tool.
- Validation Agent
Detects invoice mistakes (incorrect GST %, round-off errors, missing fields).
- Reporting Agent
Generates monthly summaries, top vendors, top items, revenue, GST liability, and anomalies.
Agents communicate sequentially and in parallel using A2A messages.

2. Tools (Custom, Code Execution, Search)
The project integrates multiple tool types:
- Custom “BulkExcelImporter” tool
Reads enterprise-provided Excel sheets (.xlsx) and converts them to transactions.
-  Code Execution Tool
Used by the GST Calculator Agent for accurate arithmetic:
GST = taxableAmt × rate / 100
CGST/SGST split
Round-off
- Search Tool (Google Search)
Fetches GST rate changes, item HSN classification info.
- PDF Parsing Tool (optional)
Converts invoice PDFs into text for the extraction agent.

3. Sessions & Memory
- Session State (InMemorySessionService) keeps:
recent invoices, current reporting cycle, previous queries.
- Memory Bank (Long-term Memory) stores:
recurring vendors
frequently purchased items
GST rates for certain HSN codes
customer's business profile (company name, state)
This improves personalization and accuracy.

4. Long-running Operations
For large invoice batches:
- The agent pauses processing
- Stores progress
- Resumes when user returns
- Provides checkpoints
This ensures enterprise-scale reliability.

5. Context Engineering
To handle dozens of invoices:
- Dynamically compacts past conversation state
- Removes irrelevant parts
- Retains invoice summaries, not raw text
- This ensures efficient context window usage during bulk imports.

6. Observability
Complete Observability Layer:
1. Logging
Each agent logs inputs, outputs, validation errors.
2.Tracing
End-to-end trace: Invoice → Extraction → GST Calc → Validation → Report.
3.Metrics
- number of invoices processed
- processing time
- GST mismatch count
- accuracy of extractions
- monthly revenue bar chart

7. Agent Evaluation
The system includes a scenario-based evaluation suite:
- Accuracy of extraction
- GST correctness
- Error cross-checking
- Monthly report stability
- Robustness to incomplete invoices

8. Deployment
The final system is deployed as:

A REST API
with endpoints for:
/uploadInvoice
/importExcel
/calculateGST
/generateReport

A UI Dashboard (Streamlit/Kaggle)
showing:
invoices
errors
GST breakdown
charts
logs & traces


Features & Benefits
Core Features

- Extracts invoice details (items, taxes, totals)
- Calculates CGST / SGST / IGST
- Excel bulk import
- Detects GST mismatches
- Generates monthly summary reports
- Shows analytics dashboard
- Stores business profile in memory
- Supports 100+ invoices at once

Business Benefits
- Reduces 70% manual billing workload
- Eliminates GST calculation mistakes
- Improves audit readiness
- Saves time on compliance
- Ensures transparency with logs & metrics


6. Agent Workflow (Detailed)
Step 1: User uploads invoice or Excel
→ Orchestrator Agent triggers parsing
Step 2: Extraction Agent
- Uses OCR/PDF tool
- Converts to structured invoice JSON
Step 3: GST Classification Agent
- Detects correct GST type based on state
-Uses memory for known vendors
Step 4: GST Calculation Agent
- Executes precise arithmetic
- Adds tax fields to invoice
Step 5: Validation Agent
- Flags errors
- Logs inconsistencies
Step 6: Reporting Agent
- Updates monthly summary
- Generates charts and insights

Workflow Diagram 

<img width="298" height="512" alt="Screenshot 2025-11-15 122920" src="https://github.com/user-attachments/assets/0b1c23fe-1cd0-438c-a06b-f8ec1a544d0b" />


Architecture Diagram

<img width="495" height="326" alt="Screenshot 2025-11-15 120635" src="https://github.com/user-attachments/assets/7075c3f7-1f36-4e98-938a-9af146aff87f" />








