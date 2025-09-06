# ROY: Research Object YAML  

*A Human-Friendly Alternative to RO-Crate JSON-LD*  

---

## 1. Motivation  

Research Objects (ROs) are a powerful way to capture the context around data, code, and workflows. The [RO-Crate](https://www.researchobject.org/ro-crate/) specification, built on JSON-LD, provides a machine-actionable standard for describing these digital artifacts.  

But for many researchers, JSON-LD can feel overwhelming:  

- **Verbosity**: Triples with long IRIs dominate the view.  
- **Cognitive overhead**: Even simple relationships (`creator`, `license`) are buried in `@id` scaffolding.  
- **Git-unfriendly**: Small edits cause large diffs because of nested structures.  

Enter **ROY — Research Object YAML**.  

ROY is a **compressed, human-first representation** of RO-Crates in YAML. It preserves all identifiers needed to reconstruct the original JSON-LD, but it presents them in a way that feels closer to a datasheet or a BibTeX entry.  

---

## 2. Objectives  

ROY is guided by three objectives:  

1. **Readability**: Make research object metadata instantly scannable by humans.  
2. **Compactness**: Strip structural noise while retaining semantics.  
3. **Round-trippability**: Ensure every ROY document can be faithfully expanded back into RO-Crate JSON-LD.  

---

## 3. Design Principles  

### 3.1 Inline Identifiers  

Use the **email/citation idiom**:  

```yaml
Alice Brown <#alice>
CC-BY-NC-SA-4.0 <https://spdx.org/licenses/CC-BY-NC-SA-4.0>
```

Names come first, identifiers remain in `<...>` so nothing is lost.  

---

### 3.2 Flatten Singletons  

If a dictionary has only one entry, collapse it:  

```yaml
people.alice: Alice Brown <#alice>
orgs.workflow-hub: Example Workflow Hub <http://example.com/workflows/>
```

This avoids unnecessary nesting while keeping IDs clear.  

---

### 3.3 Collapse Lists  

If a list has a single item, store it as a scalar:  

```yaml
parts: workflow/alignment.knime
```

This reflects the intuitive expectation: one thing → one line.  

---

### 3.4 Dot Notation for Hierarchy  

Hierarchical information is expressed through dot notation:  

```yaml
workflows.alignment.name: Sequence alignment workflow
workflows.alignment.language: KNIME (4.1.3) <https://www.knime.com/whats-new-in-knime-41>
```

This mirrors how people already think about object paths (`workflow.alignment`).  

---

## 4. Example  

### 4.1 Original RO-Crate JSON-LD (fragment)  

```json
{
  "@context": {
    "schema": "<http://schema.org/>",
    "spdx": "<https://spdx.org/licenses/>"
  },
  "@graph": [
    {
      "@id": "workflow/alignment.knime",
      "@type": [
        "schema:File",
        "schema:SoftwareSourceCode",
        "schema:ComputationalWorkflow"
      ],
      "schema:name": "Sequence alignment workflow",
      "schema:creator": { "@id": "#alice" },
      "schema:license": { "@id": "https://spdx.org/licenses/CC-BY-NC-SA-4.0" }
    },
    {
      "@id": "#alice",
      "@type": "schema:Person",
      "schema:name": "Alice Brown"
    },
    {
      "@id": "https://spdx.org/licenses/CC-BY-NC-SA-4.0",
      "@type": "schema:CreativeWork",
      "schema:name": "Creative Commons Attribution Non Commercial Share Alike 4.0 International",
      "schema:alternateName": "CC-BY-NC-SA-4.0"
    }
  ]
}
```

---

### 4.2 Equivalent ROY  

```yaml
workflows.alignment:
  type: [File, SoftwareSourceCode, ComputationalWorkflow]
  name: Sequence alignment workflow
  creator: Alice Brown <#alice>
  license: CC-BY-NC-SA-4.0 <https://spdx.org/licenses/CC-BY-NC-SA-4.0>

people.alice: Alice Brown <#alice>
```

Notice how:  

- The **workflow** has a friendly name with inline identifiers.  
- The **creator** resolves directly to a person entry.  
- The **license** shows both its shorthand and canonical URI.  

---

## 5. Benefits  

- **Human-Friendly**: A researcher can open ROY in any text editor and immediately understand the structure.  
- **Git-Friendly**: Changes are small, localized, and diffable.  
- **Flexible**: Can be expanded back into RO-Crate JSON-LD for machine use.  
- **Extensible**: Dot notation and inline `<ref>`s make it easy to add domain-specific vocabularies without bloating the view.  

---

## 6. Future Directions  

- **Tooling**: Simple Python/TypeScript converters between JSON-LD and ROY.  
- **Schemas**: Lightweight YAML/JSON Schema to validate ROY files.  
- **Profiles**: Domain-specific conventions (e.g., workflows, datasets, instruments).  
- **Community Feedback**: Iterate with real-world adoption to refine compression rules.  

---

## 7. Conclusion  

ROY is not a replacement for RO-Crate — it’s a **companion format**. While RO-Crate focuses on machine interoperability, ROY focuses on making the information easy for people to read and work with.  

Think of it like a handwritten note versus a formal printed report:  

- The printed report (RO-Crate JSON-LD) is detailed and structured for universal understanding.  
- The handwritten note (ROY) is simpler, friendlier, and still conveys all the key information.  

By introducing ROY, we give researchers a more approachable way to **author, review, and version control** their research metadata — without losing the rigor of linked data.
