---
name: secfeed
description: Answer questions with SEC EDGAR data (filings, insider trades, 13F holdings, financial statements, 8-K events, executive pay) using the SecFeed MCP tools. Use when the user asks about a US public company's filings, insiders, institutional owners or financials.
---

# Using SecFeed

SecFeed is a hosted MCP server at `https://secfeedapi.com/mcp`. Listing its tools needs no key; calling them needs the header `Authorization: Bearer <key>`.

## Setup

If a tool call returns 401, the API key is missing. Ask the user for their SecFeed key (create one at https://secfeedapi.com/signup/, copy it from https://secfeedapi.com/dashboard/) and add it as an `Authorization: Bearer <key>` header on the `secfeed` server in the client's MCP settings. Never write the key into files that get committed.

## Picking a tool

* Company identifiers first: `map_ticker` (ticker to CIK) or `map_cusip`.
* Filings: `search_filings` with a query such as `ticker:AAPL AND formType:"10-K"`; `full_text_search` for phrases inside filings.
* Text of a section: `extract_section` (10-K item 1A is Risk Factors, item 7 is MD&A).
* Numbers: `financial_statements` for the latest statements, `xbrl_to_json` for one filing, `company_facts` for full history (large).
* Insiders: `insider_trades` (Form 3/4/5). Transaction code P is an open market purchase, S a sale.
* Funds: `form_13f_holdings` by CUSIP (who owns a stock) or by filer CIK (what a fund owns).
* Events: `events_8k` by item, e.g. 5.02 leadership changes, 4.02 restatements, 1.05 cyber incidents.
* Proxy data: `executive_compensation`, `directors`, `pay_vs_performance`.

## Good habits

* Cite the filing (form type, filing date, accession number) for every number you report.
* Ask for small pages (`size`) first; widen only if needed.
* A 429 means the plan's quota is used up; tell the user rather than retrying.
