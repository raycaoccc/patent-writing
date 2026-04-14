# Prior Art Search Reference

## Search Types

| Type | Purpose | When to Conduct |
|------|---------|----------------|
| **Patentability/Novelty** | Is the invention new and non-obvious? | Before filing |
| **Freedom-to-Operate (FTO)** | Does our product infringe existing patents? | Before commercialization |
| **Invalidity** | Find prior art to challenge an existing patent | During disputes |
| **Landscape/State-of-Art** | Map technology trends and white spaces | Strategic planning |

## Search Databases

### Primary Databases

| Database | URL | Coverage | Best For |
|----------|-----|----------|----------|
| Google Patents | patents.google.com | 87M+ from 17 offices | Full-text search, broad discovery |
| Espacenet | worldwide.espacenet.com | 150M+ from 100+ countries | Citation analysis, patent families |
| USPTO PatFT/AppFT | patft.uspto.gov | US patents + applications | US-specific deep search |
| CNIPA | pss-system.cponline.cnipa.gov.cn | Chinese patents + utility models | Chinese prior art |
| WIPO PATENTSCOPE | patentscope.wipo.int | International PCT applications | International filings |

### Non-Patent Literature (NPL) Sources

| Source | Coverage | Critical For |
|--------|----------|-------------|
| arXiv | Preprints | AI/ML prior art (often predates patents) |
| IEEE Xplore | Published papers | Engineering implementations |
| ACM Digital Library | CS papers | Software/algorithm prior art |
| Semantic Scholar | Cross-domain | Citation network analysis |
| GitHub (with timestamps) | Open source | Software prior art with dates |

## 6-Phase Search Framework

### Phase 1: Keyword Search

1. **Decompose the invention** into core technical concepts
2. **Build keyword sets** for each concept:
   - Primary terms, synonyms, variants
   - Domain-specific jargon
   - Abbreviations and acronyms
3. **Construct Boolean queries**:
   ```
   ("neural network" OR "deep learning" OR "machine learning")
   AND ("text classification" OR "document categorization")
   AND ("attention mechanism" OR "transformer")
   ```
4. **Search in different fields separately**: title, abstract, claims, full text
5. **Document ALL keywords tested**

### Phase 2: Classification Search

Use CPC codes (see `cpc-codes-guide.md` for full table):
1. Find CPC codes for your technology area
2. Search within those classifications
3. Combine classification + keywords for precision

### Phase 3: Citation Analysis

1. **Forward citations**: Who cited the key patents? (follow-on work)
2. **Backward citations**: What did key patents cite? (foundational art)
3. **Examiner citations** are especially valuable
4. Build a citation network from the most relevant references

### Phase 4: Patent Family Analysis

- Track the same invention across jurisdictions
- Reveals market priorities and claim scope variations
- Use Espacenet INPADOC family view
- Check for abandoned or expired family members

### Phase 5: Non-Patent Literature Search

Critical for AI/ML inventions:
1. Search arXiv for preprints with relevant keywords
2. Check conference proceedings (NeurIPS, ICML, ICLR, ACL, CVPR)
3. Search GitHub for open-source implementations with commit dates
4. Review technical standards documents
5. Check dissertations and theses

### Phase 6: Documentation

Record for every search:
- Database searched, date of search
- Keywords and Boolean queries used
- Classification codes used
- Date range covered
- Number of results reviewed
- Key references found

## Search Report Template

```markdown
# Prior Art Search Report

## Invention Summary
[Brief description of the invention]

## Search Parameters
- **Date conducted**: YYYY-MM-DD
- **Databases searched**: [list]
- **Date range**: [range]
- **Classification codes**: [CPC codes]

## Keywords Used
| Concept | Keywords |
|---------|----------|
| Concept A | term1, term2, term3 |
| Concept B | term1, term2 |

## Key References Found

### Most Relevant (Directly Related)
1. [Patent/Paper] - [Title] - [Date] - [Relevance]

### Moderately Relevant (Overlapping Features)
1. ...

### Background Art (General Domain)
1. ...

## Novelty Assessment
- **Novel features**: [features not found in prior art]
- **Known features**: [features found in prior art]
- **Potential issues**: [areas of concern]

## Recommendations
- [Filing strategy recommendations]
- [Claim scope suggestions based on prior art]
```

## AI/ML Prior Art Tips

1. **arXiv dates matter**: Many ML innovations appear on arXiv months/years before any patent filing
2. **Check GitHub repos**: Open-source implementations with commit timestamps can be prior art
3. **Survey papers are goldmines**: They often cite dozens of relevant references
4. **Blog posts count**: Technical blog posts with publication dates qualify as prior art
5. **Competition solutions**: Kaggle writeups can establish prior use
6. **Conference workshops**: Workshop papers often precede main conference publications

## Common Pitfalls

- Stopping too early (need multiple iterations with different keywords)
- Ignoring NPL (for AI, often more relevant than patents)
- Using only one CPC code (misses cross-domain inventions)
- Language bias (missing Chinese, Japanese, Korean prior art)
- Date blindness (not checking if prior art predates priority date)
