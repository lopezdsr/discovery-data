---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-29"

subcollection: discovery-data

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:important: .important}
{:deprecated: .deprecated}
{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:hide-dashboard: .hide-dashboard}
{:apikey: data-credential-placeholder='apikey'} 
{:url: data-credential-placeholder='url'}
{:curl: .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:swift: .ph data-hd-programlang='swift'}
{:go: .ph data-hd-programlang='go'}

# Default project settings
{: #project-defaults}

To learn more about projects, see [Creating projects](/docs/discovery-data?topic=discovery-data-projects).

## Basic project defaults
{: #basic-defaults}

| Default | Document Retrieval | Document Retrieval<br/>(Content Intelligence) |
| --- | --- | --- |
| CSV settings | NA  | NA  |
| Enrichments | Entities, Parts of speech  | Entities, Parts of speech, Table Understanding, and Contracts |
| Improvement tools | Facets (by Entity), Dynamic Facets, Passages | Facets (by Category, Nature, Contract Term, Contract Payment Term, Contract Type, Contract Currency, Invoice Buyer, Invoice supplier, Invoice Currency, Purchase Order Buyer, Purchase Order Supplier, Purchase Order Payment Term) and Table Retrieval |
| Optical character recognition | off | on |
{: caption="Basic project defaults" caption-side="top"}

## Basic project defaults continued
{: #basic-defaults-cont}

| Default | Conversational search | Content Mining | Custom |
| --- | --- | --- | --- |
| CSV settings | NA | No header, selected delimiters are comma and semicolon | NA |
| Enrichments | None | Parts of speech | None |
| Improvement tools | Passages | None | Passages |
| Optical character recognition | off | off | off |
{: caption="Basic project defaults" caption-side="top"}

## Default query settings
{: #query-defaults}

| Query default | Document Retrieval | Document Retrieval<br/>(Content Intelligence) |
| --- | --- | --- |
| Aggregation<br/>(`aggregation`) | "term(enriched_text.entities.text,<br/>name:entities)"  | "[term<br/>(enriched_html.contract.<br/>elements.categories.label,<br/>count:25,name:categories)" |
| Count<br/>(`count`) | 10  | 10  |
| Find answers<br/>Enabled<br/>(`find_answers.enabled`) | false | false |
| Find answers<br/>Max answers<br/> per passage<br/>(`max_answers_per_passage`) | 1 | 1 |
| Highlight<br/>(`highlight`) | false | false |
| Passages<br/>Characters<br/>(`passages.characters`) | 200 | 200 |
| Passages<br/>Count<br/>(`passages.count`)| 10 | 10 |
| Passages<br/>Enabled<br/>(`passages.enabled`) | true | true |
| Passages<br/>Fields<br/>(`passages.fields`) | ["text", "title"] | ["text", "title"] |
| Passages<br/>Max per<br/> document<br/>(`passages.max_per_document`) | 1 | 1 |
| Passages<br/>Per document<br/>(`passages.per_document`) | true | true |
| Return<br/>(`return`)| [] | [] |
| Spelling<br/>suggestions<br/>(`spelling_suggestions`) | true | true |
| Sort<br/>(`sort`) | "" | "" |
| Suggested<br/>refinements<br/>Count<br/>(`suggested_refinements.count`) | 10 | 10 |
| Suggested<br/>refinements<br/>Enabled<br/>(`suggested_refinements.enabled`) | false | false |
| Table results<br/>Count<br/>(`table_results.count`)  | 10 | 10 |
| Table results<br/>Enabled<br/>(`table_results.enabled`) | false | true |
| Table results<br/>Per document<br/> (`table_results.per_document`) | 0 | 0 |
{: caption="Default query settings" caption-side="top"}

## Default query settings continued
{: #query-defaults-cont}

| Query default | Conversational Search | Content Mining | Custom |
| --- | --- | --- | --- |
| Aggregation<br/>(`aggregation`) | "" | "" | "" |
| Count<br/>(`count`) | 10  | 10 |  10 |
| Find answers<br/>Enabled<br/>(`find_answers.enabled`) | false | false | false |
| Find answers<br/>Max answers<br/> per passage<br/>(`max_answers_per_passage`) | 1 | 1 | 1 |
| Highlight<br/>(`highlight`) | false | false | false |
| Passages<br/>Characters<br/>(`passages.characters`)  | 200  | 200 | 200 |
| Passages<br/>Count<br/>(`passages.count`) | 10 | 10 | 10 |
| Passages<br/>Enabled<br/>(`passages.enabled`) | true | false | true |
| Passages<br/>Fields<br/>(`passages.fields`) | ["text", "title"] | ["text", "title"] | ["text", "title"] |
| Passages<br/>Max per<br/> document<br/>(`passages.max_per_document`) | 1 | 1 | 1 |
| Passages<br/>Per document<br/>(`passages.per_document`) | true | true | true |
| Return<br/>(`return`)| [] | [] | [] |
| Spelling<br/>suggestions<br/>(`spelling_suggestions`) | false | true | true |
| Sort<br/>(`sort`) | "" | "" | "" |
| Suggested<br/>refinements<br/>Count<br/>(`suggested_refinements.count`) | 10 | 10 | 10 |
| Suggested<br/>refinements<br/>Enabled<br/>(`suggested_refinements.enabled`) | false | false | false |
| Table results<br/>Count<br/>(`table_results.count`) | 10 | 10 | 10 |
| Table results<br/>Enabled<br/>(`table_results.enabled`)| false | false | false |
| Table results<br/>Per document<br/> (`table_results.per_document`) | 0 | 0 | 0 |
{: caption="Default query settings continued" caption-side="top"}

## Project component settings
{: #component-defaults}

| Default | Document Retrieval | Document Retrieval (Content Intelligence) |
| --- | --- | --- |
| Aggregations<br/>(`aggregations.name`)<br/>(`aggregations.label`)<br/>(`aggregations.`<br/>`multiple_selections_allowed`) | "name": "entities"<br/> "label": "Top Entities"<br/> "multiple_selections_allowed": false <br/> <br/>"name": "_system_suggested_refinements"<br/> "label": "Dynamic Facets"<br/>"multiple_selections_allowed": true <br/><br/>"name": "_system_collections"<br/>"label": "Collections"<br/>"multiple_selections_allowed": true | "name": "categories"<br/> "label": "Category"<br/> "multiple_selections_allowed": true<br/><br/> "name": "natures"<br/> "label": "Nature"<br/>"multiple_selections_allowed": false<br/><br/>"name": "contract_terms"<br/> "label": "Contract Term"<br/> "multiple_selections_allowed": false<br/><br/>"name": "contract_payment_terms"<br/>"label": "Contract Payment Term"<br/>"multiple_selections_allowed": false<br/> <br/>"name": "contract_types"<br/> "label": "Contract Type"<br/> "multiple_selections_allowed": false<br/><br/> "name": "contract_currencies"<br/> "label": "Contract Currency"<br/>"multiple_selections_allowed": false<br/><br/>"name": "invoice_buyers"<br/> "label": "Invoice Buyer"<br/> "multiple_selections_allowed": false<br/><br/>"name": "invoice_suppliers"<br/> "label": "Invoice Supplier"<br/> "multiple_selections_allowed": false<br/><br/>"name": "invoice_currencies"<br/> "label": "Invoice Currency"<br/> "multiple_selections_allowed": false<br/><br/>"name": "po_payment_terms"<br/> "label": "Purchase Order Payment Term"<br/> "multiple_selections_allowed": false<br/><br/>"name": "po_buyers"<br/> "label": "Purchase Order Buyer"<br/> "multiple_selections_allowed": false<br/><br/>"name": "po_suppliers"<br/> "label": "Purchase Order Supplier"<br/> "multiple_selections_allowed": false<br/><br/> "name": "po_currencies"<br/>"label": "Purchase Order Currency"<br/>"multiple_selections_allowed": false |
| Autocomplete<br/>(`autocomplete`) | true | true |
| Fields shown<br/>Body field<br/> (`fields_shown.body.field`)| "" |  "" |
| Fields shown<br/>Body Use passage<br/> (`fields_shown.body.use_passage`) | true  | true |
| Fields shown<br/>Title/field<br/> (`fields_shown.title.field`) | "title" | "title" |
| Results per page<br/> (`results_per_page`) | 5  | 5 |
| Structured search<br/>(`structured_search`) | false  | false |
{: caption="Project component settings" caption-side="top"}

## Project component settings continued
{: #component-defaults-cont}

The Custom project type has no project component default settings.

| Default | Conversational search | Content Mining |
| --- | --- | --- |
| Aggregations<br/>(`aggregations.name`)<br/>(`aggregations.label`)<br/>(`aggregations.`<br/>`multiple_selections_allowed`) | [] | [] |
| Autocomplete<br/>(`autocomplete`) | false | true |
| Fields shown<br/>Body field<br/> (`fields_shown.body.field`)| "" | text |
| Fields shown<br/>Body Use passage<br/> (`fields_shown.body.use_passage`) | true  | false |
| Fields shown<br/>Title/field<br/> (`fields_shown.title.field`) | "title" | "document_id" |
| Results per page<br/> (`results_per_page`) | 0 | 0 |
| Structured search<br/>(`structured_search`) | false  | false |
{: caption="Project component settings continued" caption-side="top"}

## API reference
{: #project-defaults-api}

For API reference information, see the following documentation:

- [Create a project method](https://{DomainName}/apidocs/discovery-data#createproject){: external}
- [Get configuration settings for components method](https://{DomainName}/apidocs/discovery-data#getcomponentsettings){: external}
