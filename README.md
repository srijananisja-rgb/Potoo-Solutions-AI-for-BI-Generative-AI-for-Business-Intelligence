# Potoo Solutions' AI for BI — Generative AI for Business Intelligence

A Generative AI prototype that transforms static Tableau dashboards into dynamic, conversational business intelligence interfaces, built for OPIM 5894 (Generative AI in Business) in collaboration with Potoo Solutions.

**Team 5:** Likhitha Guggilla, Nishita Rao, Srijanani Achuthan, Vinay Kiran Reddy Chinnakondu

---

## Executive Summary

This project enhances business intelligence by replacing static dashboards with conversational, LLM-powered interfaces that allow users to query structured data in natural language and receive real-time text and visual responses. Two distinct workflows were developed and evaluated using Potoo Solutions' experimental data for the **Wüsthof** brand: an **Agentic Workflow** that connects to live API data, and a **Simple Workflow** that processes structured Excel data to generate Python-based visualizations. The Agentic Workflow excelled at real-time data handling, while the Simple Workflow performed best for visual outputs.

---

## Project Goals

- Transform static Tableau dashboards into dynamic, conversational BI interfaces
- Enable users to query a structured database directly using natural language
- Leverage LLMs to provide real-time, accurate insights in both text and visual formats
- Deliver a scalable prototype demonstrating the value of AI-driven analytics for Potoo Solutions' customers

---

## Scope & Boundaries

- Focused on building a functional prototype using Potoo Solutions' demo brand, **Wüsthof**
- The system answers a predefined set of preliminary questions developed by the team
- Real-time customer queries and broader business use cases were intentionally excluded to keep the prototype focused and efficient
- Designed as a controlled environment for validating the approach, with room for future expansion

---

## Data

- **Format:** Available via MS Excel (.xlsx) and live API access
- **Content:** Inventory details for Potoo Solutions' experimental brand, Wüsthof
- **Variables of interest:** Date, SKU, Product Name, UPC, Retailer Item Price
- **Test data range:** January 1, 2024 (560 rows × 5 columns), limited to manage LLM token constraints

---

## GenAI Application Design

Two workflows were built using **Dify** to parse structured data and answer user queries:

### Approach 1: Agentic Workflow (Real-Time API)

A multi-node workflow that connects to Potoo Solutions' live API:

1. **Start node** — collects required inputs: `start_date`, `end_date`, `brand`, and `user question`
2. **HTTP Request node** — sends a GET request to Potoo's API endpoint to retrieve experimental Wüsthof data in JSON format
3. **JSON Parse node** — filters and formats the API response for LLM processing
4. **LLM node (GPT-4o)** — generates responses, prompted to act as a **Data Analyst Assistant**
5. **End node** — outputs the generated text as a string

This HTTP workflow was then wrapped into a **conversational agentic system** with memory retention. The chatbot prompts users for their question, date range, and brand of interest, matching these to the Start node's required inputs and triggering the workflow. The agent is prompted to act as an experienced data analyst capable of full inventory data analysis.

**Demonstration:** The chatbot successfully identified all unique prices at which UPC `4002293120782` (Wüsthof Classic 2-Piece Chef Set) was sold between 2024-01-01 and 2024-01-01, with responses verified against the raw data using Python.

### Approach 2: Simple Workflow (File Upload + Visualization)

A workflow designed for visual generation using sample Excel data:

1. **Start node** — requires a structured Excel file (`Potoo_Sample_Cleaned_Data.xlsx`) and a user question
2. **Doc Extractor node** — converts the structured data into text format
3. **LLM node (GPT-4o)** — generates responses/code based on the extracted data, prompted as a **Data Analyst Assistant**
4. **Code Interpreter node** — executes the generated Python code
5. **End node** — outputs the resulting code/visualization

**Demonstration:** Given the query *"How is the price trend for the brand 'Wusthof' in September 2024?"*, the workflow generated Python code (pandas + matplotlib) that correctly produced a daily average price trend plot, accurately reflecting the underlying Excel data.

---

## Evaluation Methodology

Three workflow types were evaluated across three metrics:

| Metric | Description |
|---|---|
| **Accuracy Score** | Correctness of extracted data, code execution without errors, and accuracy of generated visualizations (axes, labels, alignment with query) |
| **Faithfulness** | Whether LLM responses are factually correct and free of hallucination |
| **Answer Relevancy** | Whether the LLM retains context and correctly understands the user's query |

### Results

| System Type | Accuracy Score | Faithfulness | Answer Relevancy |
|---|---|---|---|
| **Retrieval Augmented Generation (RAG)** | Low | Low | Low |
| **Workflow with File Upload + Prompt Engineering** | Moderate | High | High |
| **Agentic System (API data) + Prompt Engineering** | Moderate–High | High | High |

**Key finding:** RAG struggled across all metrics due to difficulty processing structured data. Both the **Workflow with File Upload** and **Agentic System** performed well, with the Agentic System showing slightly better accuracy due to efficient real-time data retrieval. Well-designed prompts and clear workflow structure were critical to achieving reliable results.

---

## Challenges & Limitations

- **Token limits:** LLMs struggled to process large volumes of data due to input token constraints, requiring data to be limited to a single day (560 rows × 5 columns) for testing
- **Response consistency:** Even with refined prompting, LLM outputs showed variability, highlighting the need for further fine-tuning or external logic layers for high-accuracy tasks
- **Platform limitations:** Dify offered a user-friendly, low-code environment but lacked pre-installed Python modules, slowing down certain processes

---

## Future Work

- Scale to larger, real-time API-accessed datasets with optimized processing strategies
- Extend LLM capabilities to generate data visualizations directly within workflows/chatflows
- Incorporate Potoo Solutions' real-time customer query documentation to improve support and response accuracy

---

## Tools & Technologies

- **Dify** — Low-code platform for building LLM workflows and agentic systems
- **GPT-4o** — Core LLM for natural language understanding and response generation
- **Python** (pandas, matplotlib) — Data analysis and visualization via Code Interpreter
- **REST API / HTTP Request, JSON Parsing** — Real-time data access
- **MS Excel** — Structured sample data source

---

## References

1. [Potoo Solutions](https://potoosolutions.com/)
2. [Dify: JSON Schema Usage](https://docs.dify.ai/learn-more/extended-reading/how-to-use-json-schema-in-dify)
3. [Dify: HTTP Request Node](https://docs.dify.ai/guides/workflow/node/http-request)
4. [Dify: Code Node](https://docs.dify.ai/guides/workflow/node/code)
