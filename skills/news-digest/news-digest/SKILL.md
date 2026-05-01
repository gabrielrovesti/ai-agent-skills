---
name: news-digest
description: >
  Produce a comprehensive, structured, multi-section news digest by searching the web
  in real time across the widest possible range of source types (wire agencies, newspapers,
  trade press, academic preprints, institutional sources, community forums, social signals,
  financial data, podcasts). Use this skill whenever the user asks for news, a daily briefing,
  "cosa succede oggi", "ultime notizie", "news di oggi", "rassegna stampa", "aggiornamenti
  tech/informatica", "dimmi le news", "cosa è successo", "catch me up", "daily digest",
  "news roundup", or any variation. Trigger even for partial or casual requests — if the
  user wants to know what's happening in the world, in Italy, in tech, in science, in
  markets, or in any specific domain, this skill applies. Always use this skill over
  answering from memory when current events are involved.
---

# News Digest Skill — Extended Edition

Produce a **structured, multi-section news digest** with three mandatory sections and
several optional sections selectable by the user. Default output covers all three mandatory
sections. Optional sections are added when the user specifies a domain or asks for
a "full" / "complete" / "extended" digest.

---

## Section Map

### Mandatory (always included)

| # | Section | Emoji | Scope |
|---|---------|-------|-------|
| 1 | Italia | 🇮🇹 | Domestic Italian news: politics, economy, law, society, culture, environment |
| 2 | Estero | 🌍 | International: geopolitics, conflicts, diplomacy, major world events |
| 3 | Informatica & Tech | 💻 | IT, CS, AI/ML, cybersecurity, software, hardware, cloud, open source, research |

### Optional (add when requested or when producing a "full digest")

| # | Section | Emoji | Scope |
|---|---------|-------|-------|
| 4 | Scienza & Ricerca | 🔬 | Physics, biology, medicine, space, climate, academic publications |
| 5 | Economia & Mercati | 📈 | Markets, macro, central banks, M&A, commodities, crypto |
| 6 | Europa & Istituzioni | 🇪🇺 | EU legislation, ECB, Parlamento Europeo, GDPR, AI Act, regulatory |
| 7 | Sicurezza & Difesa | 🛡️ | Military, intelligence, cybersecurity geopolitics, NATO, defense industry |
| 8 | Ambiente & Energia | 🌱 | Climate, energy transition, renewables, environmental policy |
| 9 | Sport | ⚽ | Major sporting events, results, transfers (add only if explicitly requested) |
| 10 | Cultura & Società | 🎭 | Cinema, publishing, music industry, social trends, design |

---

## Search Strategy

### Execution order

1. Run all mandatory-section queries in parallel (or as close to parallel as possible).
2. Fetch full article content with `web_fetch` for any result where the snippet is <2 sentences or clearly truncated.
3. If a section yields fewer than 3 usable items after initial queries, run 1–2 additional targeted queries before giving up.
4. For optional sections: run queries only after mandatory sections are complete.

### Query volume per section

| Section              | Base queries | Max queries | Notes |
|----------------------|-------------|-------------|-------|
| Italia               | 3           | 5           | Vary: politics, economy, society, regional, judiciary |
| Estero               | 3           | 5           | Vary: Europe, Americas, Asia-Pacific, Middle East, Africa |
| Informatica & Tech   | 4           | 7           | Must cover AI, cybersec, and at least one of: research/OSS/hardware/cloud/policy |
| Scienza & Ricerca    | 2           | 4           | arXiv, Nature, Science, ESA, NASA, WHO |
| Economia & Mercati   | 2           | 4           | Macro, equities, commodities, crypto |
| Europa & Istituzioni | 2           | 3           | EUR-Lex, European Parliament, ECB, ENISA |
| Sicurezza & Difesa   | 2           | 4           | NATO, SIPRI, defense wire |
| Ambiente & Energia   | 2           | 3           | IEA, IPCC, renewables, climate policy |
| Sport                | 2           | 3           | Only when explicitly requested |
| Cultura & Società    | 2           | 3           | Publishing, cinema, social trends |

---

## Query Construction Rules

### Universal rules
- **Always embed a temporal anchor**: current year (`2026`), current month, or `"today"` / `"oggi"`.
- Diversify query angle: do not repeat the same framing. Rotate "latest", "breaking", "update", "announced", "report", "analysis", "published".
- When a specific domain is requested, increase query depth for that domain.

### Per-section query strategy

#### 🇮🇹 Italia
- Primary: Italian-language queries to surface Italian-press coverage.
- Cover at minimum: politics/government + economics/business + one of {judiciary, environment, social policy, culture, regional news}.
- Example queries:
  - `"notizie Italia politica oggi maggio 2026"`
  - `"economia italiana ultimi aggiornamenti 2026"`
  - `"cronaca Italia oggi"`
  - `"governo italiano ultime decisioni"`
  - `"giustizia Italia news 2026"`
  - `"notizie regionali Italia oggi"`
  - `"ANSA ultime notizie oggi"`

#### 🌍 Estero
- Primary: English; supplement with Italian for European context.
- Cover at minimum: one major geopolitical story + one economic story + one humanitarian/climate story.
- Rotate geographic focus: Europe, Americas, Asia-Pacific, Middle East, Sub-Saharan Africa.
- Example queries:
  - `"world news today May 2026"`
  - `"geopolitics latest developments 2026"`
  - `"US foreign policy news today"`
  - `"China economy news 2026"`
  - `"Middle East conflict update today"`
  - `"Africa latest news 2026"`
  - `"Latin America news today"`
  - `"UN Security Council 2026"`
  - `"G7 G20 news 2026"`

#### 💻 Informatica & Tech
Cover all five sub-buckets every session; rotate depth:

| Sub-bucket | Mandatory? | Example queries |
|---|---|---|
| AI & ML | Yes | `"AI news today 2026"`, `"LLM model release 2026"`, `"machine learning research 2026"`, `"AI regulation news"` |
| Cybersecurity | Yes | `"cybersecurity news today"`, `"new CVE 2026"`, `"ransomware 2026"`, `"data breach today"`, `"CISA advisory 2026"` |
| Software / Dev / OSS | Rotate | `"open source release 2026"`, `"GitHub trending 2026"`, `"framework library announcement 2026"`, `"developer tools news"` |
| Hardware / Semiconductors / Cloud | Rotate | `"semiconductor news 2026"`, `"cloud provider announcement today"`, `"GPU release 2026"`, `"chip fab news"` |
| Policy / Regulation | Rotate | `"EU AI Act 2026"`, `"GDPR enforcement 2026"`, `"tech antitrust news"`, `"digital regulation news"` |
| CS Research / Academia | Rotate | `"computer science research 2026"`, `"arXiv CS paper notable 2026"`, `"ACM IEEE conference paper 2026"` |

#### 🔬 Scienza & Ricerca
- Example queries:
  - `"science research news today 2026"`
  - `"Nature Science paper 2026"`
  - `"space exploration news today"`
  - `"ESA NASA update 2026"`
  - `"medical research breakthrough 2026"`
  - `"physics discovery 2026"`
  - `"climate science report 2026"`
  - `"biology genomics news 2026"`

#### 📈 Economia & Mercati
- Example queries:
  - `"market news today May 2026"`
  - `"central bank interest rate 2026"`
  - `"commodity prices update today"`
  - `"M&A deal announced 2026"`
  - `"crypto market news today"`
  - `"eurozone economy update 2026"`
  - `"inflation data 2026"`
  - `"stock market news today"`

#### 🇪🇺 Europa & Istituzioni
- Example queries:
  - `"European Parliament vote 2026"`
  - `"EU regulation news today"`
  - `"ECB announcement 2026"`
  - `"ENISA report 2026"`
  - `"EU AI Act enforcement 2026"`
  - `"European Commission decision 2026"`

#### 🛡️ Sicurezza & Difesa
- Example queries:
  - `"NATO news today 2026"`
  - `"defense industry news 2026"`
  - `"military conflict update today"`
  - `"intelligence agency news 2026"`
  - `"arms deal 2026"`
  - `"SIPRI report 2026"`

#### 🌱 Ambiente & Energia
- Example queries:
  - `"climate change news today 2026"`
  - `"renewable energy news 2026"`
  - `"IEA energy report 2026"`
  - `"COP climate policy 2026"`
  - `"oil gas price today"`
  - `"energy transition news 2026"`

---

## Source Type Diversity

When formulating queries, actively target the following **source categories** by rotating
anchor terms. Load `references/sources.md` for the full curated list when needed.

| Category | Examples | When to prioritize |
|---|---|---|
| **Wire agencies** | Reuters, AP, ANSA, AFP, dpa, Kyodo, Xinhua | Breaking news, factual baseline |
| **Daily newspapers** | Corriere, Repubblica, NYT, Guardian, FT, Le Monde, El Pais, FAZ | Context, analysis |
| **Weekly / magazines** | The Economist, Der Spiegel, L'Espresso, Internazionale | Longer analysis, trend pieces |
| **Trade press** | Ars Technica, The Register, InfoQ, STAT News, IEEE Spectrum | Domain depth |
| **Academic preprints** | arXiv (cs.*, q-bio.*, physics.*), bioRxiv, SSRN, medRxiv | Research discoveries |
| **Institutional / Gov** | EUR-Lex, ECB, WHO, NASA, ESA, CISA, NIST, Gazzetta Ufficiale | Official positions, legal |
| **Think-tanks & policy** | Brookings, ECFR, ISPI, IAI, Chatham House, RAND | Geopolitical, regulatory analysis |
| **Community & forums** | Hacker News, Reddit (r/technology, r/worldnews, r/science, r/netsec) | Early signals, practitioner opinion |
| **Financial data** | Bloomberg, FT Markets, Investing.com, CoinDesk, CoinTelegraph | Markets, macro, crypto |
| **Security feeds** | BleepingComputer, Krebs on Security, CISA advisories, NVD, Recorded Future | CVEs, incidents, threat intel |
| **Open source / Dev** | GitHub Trending, Product Hunt, npm blog, CNCF announcements, Changelog | OSS ecosystem |
| **Regulatory databases** | EUR-Lex, Federal Register, Gazzetta Ufficiale, EDPB | Legal/normative changes |
| **Social signals** | X/Twitter trending (via search), LinkedIn news | Virality, public reaction |
| **Podcast/Newsletter digests** | Morning Brew, Il Post Mattina, TLDR Newsletter (indexed) | Curated signal aggregation |
| **Scientific journals** | Nature, Science, Cell, NEJM, PNAS, PRL | Peer-reviewed research |
| **Regional Italian press** | Il Gazzettino, La Nazione, Il Messaggero, Corriere del Mezzogiorno | Sub-national Italian stories |

---

## Output Format

Produce the digest as **inline markdown** — no HTML artifacts, no file creation.

### Global Header

```
# 📰 News Digest — [Giorno, DD Mese YYYY]
> Copertura: [elenco sezioni incluse] · [N] notizie totali · [N] query eseguite
```

---

### Per-Section Structure

Section header:
```
## [Emoji] [Nome Sezione]
```

Each section: **4–6 items** (mandatory), **3–5 items** (optional).

Each item:

```markdown
### [🔴] [Titolo: conciso, informativo, max ~12 parole]

**📋 Descrizione:** Fatti essenziali in 2–3 frasi. Chi, cosa, quando, dove, come.
Linguaggio neutro e preciso. Nessun aggettivo valutativo.

**💬 Commento:** 1–3 frasi di analisi: perché conta, implicazioni a breve/medio termine,
cosa osservare. Tono autorevole, diretto. Può contenere proiezioni fondate su dati o trend.

**🔗 Fonte:** [Wire / Quotidiano / Trade press / Istituzionale / Accademico / Community / Agenzia]
```

> Note: `🔴` prefix is used only for breaking/developing stories. `🔗 Fonte` is a
> source-type label, not a URL. Do not paste raw links inline.

---

## Tone and Style Rules

- **Descrizione**: factual, compressed, third-person. Chi/cosa/quando/dove. Zero adjectives carrying judgment. Present tense for ongoing situations, past tense for concluded events.
- **Commento**: sharp and analytical. State significance directly. Avoid epistemic cowardice. Use hedges only when genuine uncertainty exists.
- **Language policy**:
  - Italia section → Italian throughout.
  - Estero section → Italian throughout; proper nouns in native form.
  - Informatica section → Italian prose; English for all technical terms, model names, tool names, CVE IDs, company names, acronyms.
  - Optional sections → Italian, same rules.
- **Length discipline**: each item 8–12 lines max. No padding. No title repetition in description.
- **Paraphrase aggressively**: never reproduce more than ~10 consecutive words from any source. Synthesize across sources where possible.

---

## Multi-Source Synthesis

For high-importance stories covered by multiple sources:
- Synthesize across at minimum 2 independent sources before writing the item.
- If sources conflict, surface the discrepancy in Commento.
- Do not cite a single community post as sole source for factual claims — corroborate with institutional or journalistic source.
- For academic items: the preprint or journal abstract is the primary source; journalistic coverage is secondary.

---

## Copyright and Hallucination Rules

- Never reproduce verbatim passages from articles.
- All factual claims must trace to real search results from the current session.
- Sparse results: reduce item count, do not invent content.
- Zero usable results for a sub-topic: omit silently, note in footer.
- No citation indices inline. Source type label provides transparency.

---

## Closing Footer

```
---
*📰 Digest generato il [data e ora]. Query eseguite: [N].
Tipologie di fonte coperte: [lista compatta].
Sezioni omesse per scarsità di dati: [lista o "nessuna"].
Per approfondire qualsiasi notizia, richiedilo esplicitamente.*
```

---

## Edge Cases and Behavioral Rules

| Situation | Behavior |
|---|---|
| "Solo tech" / "solo informatica" | One section only, 7–9 items, cover all 6 sub-buckets |
| "Full digest" / "digest completo" | All 10 sections |
| "News di [paese specifico]" | Estero section focused, 5–6 items, extra queries on that country |
| "News su [topic specifico]" | Single-topic digest; 6–8 items, maximum query depth |
| Date in the past | Add date range to all queries; note if archive coverage is limited |
| No results for a sub-topic | Skip silently, mention in footer |
| User asks to expand one item | Deep-Dive mode (see below) |
| User asks in English | Entire digest in English |
| Conflicting sources | Surface conflict in Commento, note which source classes disagree |
| Breaking/developing story | Mark with 🔴 prefix; note situation is evolving |
| Paywalled source in results | Note `[paywall]` in fonte type; paraphrase from snippet only |
| Community-only signal (HN, Reddit) | Flag as `[Community signal — non verificato da fonte primaria]` |

---

## Deep-Dive Mode

When the user asks to "approfondire" or "expand" a specific item:

1. `web_fetch` the primary source URL if available, else run 2–3 targeted queries.
2. Produce an **extended item** structured as:
   - **Contesto**: 2–4 frasi di background storico/strutturale.
   - **Sviluppi**: cronologia dei fatti recenti in ordine temporale.
   - **Analisi**: implicazioni a breve, medio, lungo termine.
   - **Voci rilevanti**: posizioni di attori chiave (governi, aziende, esperti, istituzioni).
   - **Cosa monitorare**: 3 bullet point con i prossimi indicatori/eventi da seguire.
3. Format as a standalone block, outside the digest item template.

---

## Thematic Digest Mode

When the user requests a thematic digest (e.g. "fammi un digest sull'AI", "news sulla crisi energetica"):

1. Treat the theme as a cross-sectional lens applied to all relevant sections.
2. Pull from all applicable source types (institutional, trade, academic, community).
3. Produce 8–12 items grouped by sub-theme, not by geographic section.
4. Add a **Sintesi tematica** block at the top (3–5 frasi) framing the overall state of the topic.
5. End with a **Radar**: 3 bullet points on signals to watch in the next 7–30 days.

---

## Reference Files

- `references/sources.md` — full curated source list by section and category.
  Load this file when: (a) search quality is low, (b) user requests specific outlets,
  (c) you need to target a niche domain not covered by default query strategy.
