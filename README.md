---
annotations_creators:
- crowdsourced
language_creators:
- crowdsourced
language:
- en
license:
- other
multilinguality:
- monolingual
size_categories:
- 10M<n<100M
source_datasets:
- original
task_categories:
- text-generation
- fill-mask
- text-classification
task_ids:
- language-modeling
- masked-language-modeling
- text-scoring
- topic-classification
paperswithcode_id: pubmed
pretty_name: PubMed
tags:
- citation-estimation
dataset_info:
- config_name: '2023'
  features:
  - name: MedlineCitation
    struct:
    - name: PMID
      dtype: int32
    - name: DateCompleted
      struct:
      - name: Year
        dtype: int32
      - name: Month
        dtype: int32
      - name: Day
        dtype: int32
    - name: NumberOfReferences
      dtype: int32
    - name: DateRevised
      struct:
      - name: Year
        dtype: int32
      - name: Month
        dtype: int32
      - name: Day
        dtype: int32
    - name: Article
      struct:
      - name: Abstract
        struct:
        - name: AbstractText
          sequence: string
      - name: ArticleTitle
        dtype: string
      - name: AuthorList
        struct:
        - name: Author
          sequence:
          - name: LastName
            dtype: string
          - name: ForeName
            dtype: string
          - name: Initials
            dtype: string
          - name: CollectiveName
            dtype: string
      - name: Language
        dtype: string
      - name: GrantList
        struct:
        - name: Grant
          sequence:
          - name: GrantID
            dtype: string
          - name: Agency
            dtype: string
          - name: Country
            dtype: string
      - name: PublicationTypeList
        struct:
        - name: PublicationType
          sequence: string
    - name: MedlineJournalInfo
      struct:
      - name: Country
        dtype: string
    - name: ChemicalList
      struct:
      - name: Chemical
        sequence:
        - name: RegistryNumber
          dtype: string
        - name: NameOfSubstance
          dtype: string
    - name: CitationSubset
      dtype: string
    - name: MeshHeadingList
      struct:
      - name: MeshHeading
        sequence:
        - name: DescriptorName
          dtype: string
        - name: QualifierName
          dtype: string
  - name: PubmedData
    struct:
    - name: ArticleIdList
      sequence:
      - name: ArticleId
        sequence: string
    - name: PublicationStatus
      dtype: string
    - name: History
      struct:
      - name: PubMedPubDate
        sequence:
        - name: Year
          dtype: int32
        - name: Month
          dtype: int32
        - name: Day
          dtype: int32
    - name: ReferenceList
      sequence:
      - name: Citation
        dtype: string
      - name: CitationId
        dtype: int32
  splits:
  - name: train
    num_bytes: 52199025303
    num_examples: 34960700
  download_size: 41168762331
  dataset_size: 52199025303
---

# Dataset Card for PubMed

## Table of Contents
- [Dataset Description](#dataset-description)
  - [Dataset Summary](#dataset-summary)
  - [Supported Tasks and Leaderboards](#supported-tasks-and-leaderboards)
  - [Languages](#languages)
- [Dataset Structure](#dataset-structure)
  - [Data Instances](#data-instances)
  - [Data Fields](#data-fields)
  - [Data Splits](#data-splits)
- [Dataset Creation](#dataset-creation)
  - [Curation Rationale](#curation-rationale)
  - [Source Data](#source-data)
  - [Annotations](#annotations)
  - [Personal and Sensitive Information](#personal-and-sensitive-information)
- [Considerations for Using the Data](#considerations-for-using-the-data)
  - [Social Impact of Dataset](#social-impact-of-dataset)
  - [Discussion of Biases](#discussion-of-biases)
  - [Other Known Limitations](#other-known-limitations)
- [Additional Information](#additional-information)
  - [Dataset Curators](#dataset-curators)
  - [Licensing Information](#licensing-information)
  - [Citation Information](#citation-information)
  - [Contributions](#contributions)

## Dataset Description

- **Homepage:** : [https://www.nlm.nih.gov/databases/download/pubmed_medline.html]()
- **Documentation:** : [https://www.nlm.nih.gov/databases/download/pubmed_medline_documentation.html]()
- **Repository:**
- **Paper:**
- **Leaderboard:**
- **Point of Contact:**

### Dataset Summary

NLM produces a baseline set of MEDLINE/PubMed citation records in XML format for download on an annual basis. The annual baseline is released in December of each year. Each day, NLM produces update files that include new, revised and deleted citations. See our documentation page for more information.

### Supported Tasks and Leaderboards

[More Information Needed]

### Languages

- English

## Dataset Structure

Bear in mind the data comes from XML that have various tags that are hard to reflect
in a concise JSON format. Tags and list are kind of non "natural" to XML documents
leading this library to make some choices regarding data. "Journal" info was dropped
altogether as it would have led to many fields being empty all the time.

The hierarchy is also a bit unnatural but the choice was made to keep as close as
possible to the original data for future releases that may change schema from NLM's side.

Author has been kept and contains either "ForeName", "LastName", "Initials", or "CollectiveName".
(All the fields will be present all the time, but only some will be filled)

### Data Instances

```json
{
    "MedlineCitation": {
        "PMID": 0,
        "DateCompleted": {"Year": 0, "Month": 0, "Day": 0},
        "NumberOfReferences": 0,
        "DateRevised": {"Year": 0, "Month": 0, "Day": 0},
        "Article": {
            "Abstract": {"AbstractText": "Some abstract (can be missing)" },
            "ArticleTitle": "Article title",
            "AuthorList": {"Author": [
                {"FirstName": "John", "ForeName": "Doe", "Initials": "JD", "CollectiveName": ""}
                {"CollectiveName": "The Manhattan Project", "FirstName": "", "ForeName": "", "Initials": ""}
            ]},
            "Language": "en",
            "GrantList": {
                "Grant": [],
            },
            "PublicationTypeList": {"PublicationType": []},
        },
        "MedlineJournalInfo": {"Country": "France"},
        "ChemicalList": {"Chemical": [{
            "RegistryNumber": "XX",
            "NameOfSubstance": "Methanol"
        }]},
        "CitationSubset": "AIM",
        "MeshHeadingList": {
            "MeshHeading": [],
        },
    },
    "PubmedData": {
        "ArticleIdList": {"ArticleId": "10.1002/bjs.1800650203"},
        "PublicationStatus": "ppublish",
        "History": {"PubMedPubDate": [{"Year": 0, "Month": 0, "Day": 0}]},
        "ReferenceList": [{"Citation": "Somejournal", "CitationId": 01}],
    },
}
```

### Data Fields

Main Fields will probably interest people are:

- "MedlineCitation" > "Article" > "AuthorList" > "Author"
- "MedlineCitation" > "Article" > "Abstract" > "AbstractText"
- "MedlineCitation" > "Article" > "Article Title"
- "MedlineCitation" > "ChemicalList" > "Chemical"
- "MedlineCitation" > "NumberOfReferences"

### Data Splits

There are no splits in this dataset. It is given as is.

## Dataset Creation

### Curation Rationale

[More Information Needed]

### Source Data

#### Initial Data Collection and Normalization

[https://www.nlm.nih.gov/databases/download/pubmed_medline_faq.html]()

#### Who are the source language producers?

[More Information Needed]

### Annotations

#### Annotation process

[More Information Needed]

#### Who are the annotators?

[More Information Needed]

### Personal and Sensitive Information

[More Information Needed]

## Considerations for Using the Data

### Social Impact of Dataset

[More Information Needed]

### Discussion of Biases

[More Information Needed]

### Other Known Limitations

[More Information Needed]

## Additional Information

### Dataset Curators

[More Information Needed]

### Licensing Information

[https://www.nlm.nih.gov/databases/download/terms_and_conditions.html]()

### Citation Information

[Courtesy of the U.S. National Library of Medicine](https://www.nlm.nih.gov/databases/download/terms_and_conditions.html).

### Contributions

Thanks to [@Narsil](https://github.com/Narsil) for adding this dataset.
