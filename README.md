# BiomarkerKB Knowledge Graph (KG)

## Repository Structure & Setup

Server Path: /data/shared/KG

This directory contains the BiomarkerKB Knowledge Graph (KG) setup and all supporting documentation.

- README.md – Documentation on running the KG and LLM testing performed so far.

- distribution/ and csv/ – Build configurations and files for constructing the BiomarkerKB KG.

- [Supplementary Files](https://github.com/clinical-biomarkers/biomarker-partnership/tree/main/supplementary_files):Additional resources supporting the build process.

# KG Build Process

The BiomarkerKB KG is constructed by ingesting biomarker data into the CFDE Knowledge Graph (KG) managed by the [Data Distillery team](https://ubkg.docs.xconsortia.org/basics/)

## Workflow

1. **Biomarker Data Conversion**
   - Biomarker data is first converted into the `OWLNETS_edgelist.txt` and `OWLNETS_node_metadata.txt` formats required by the CFDE KG.  
   - Conversion handled using the [nt-owlnets-kg-converter](https://github.com/clinical-biomarkers/nt-owlnets-kg-converter).
   - This converts BiomarkerKB data into Ntriples. This format can then be used to generate CSVs files needed to build neo4j knowledge graphs.
   - Generated files are available in:  
     ```
     /data/shared/repos/ubkg-etl
     ```

2.  **Converting Ntriples into CSVs**
   - Biomarker Ntriples are formatted into CSVs for neo4j knowledge graph
   - The generation framework is a suite of Extract-Transform-Load (ETL) scripts that
      - extract assertion data from sources, including OWL files
      - appends assertions to the set of UMLS CSV
   - **This should allow for an internal BiomarkerKB KG to be developed**
   - Generation framework instructions are provided by the UBKG team [UBKG ETL](https://github.com/x-atlas-consortia/ubkg-etl).
   - Repo availabe in KVM2 server:
     ```
     /data/shared/repos/ubkg-etl
     ```

3.   **Integration into CFDE KG**
   - Once generated, these files are re-ingested into the CFDE KG.  
   - Full build and integration instructions are provided by the UBKG team:  
     [UBKG Build Instructions](https://github.com/x-atlas-consortia/ubkg-neo4j/blob/main/docs/BUILD_INSTRUCTIONS.md).
   - Repo availabe in KVM2 server:
     ```
     /data/shared/repos/ubkg-neo4j
     ```
     
4. **Data Updates**
   - Each time new biomarker data is pushed, the edge list and node files must be **recreated and re-ingested** into the CFDE KG.
  
## Notes & Requirements  

- Building the KG requires significant disk space:  
  > **Free disk space equal to 3–4× the size of the ontology CSVs is recommended.**

- Repository copies and scripts are stored here:
  ```
  /data/shared/repos/ubkg-neo4j
  ```
- Supplementary files are available in the [biomarker-partnership repo](https://github.com/clinical-biomarkers/biomarker-partnership/tree/main/supplementary_files).

## BiomarkerKG Use Cases

1. **Identify multi-disease biomarkers**
   - Retrieve biomarkers that are associated with multiple diseases, enabling the identification of shared biological indicators across disease domains.
2. **Identify multi-biomarker diseases**
   - Find diseases linked to multiple biomarkers to reveal complex molecular signatures and potential diagnostic panels.
3. **Detect biomarkers with multiple measurement contexts**
   - Query biomarkers that have distinct LOINC and UBERON identifiers.
   - This highlights biomarkers measurable in different specimen types or anatomical locations, suggesting disease-specific measurement modalities.

## CFDE Knowledge Graph (KG) Use Cases

1. **Explore drug target and therapy connections**
   - From a single biomarker entry, expand to identify related drug targets, therapeutic agents, and associated treatment contexts.
2. **Cross-disease treatment discovery**
   - Starting from one biomarker–disease association, identify drug therapies linked to that biomarker and expand to find other diseases treated by the same therapy, revealing shared treatment and molecular pathways.

## RENCI Knowledge Graph (KG) Use Cases

1. **Link metabolite biomarkers to protein sequences**
   - Connect metabolite biomarkers with related protein sequences to uncover molecular mechanisms and enzymatic interactions.
2. **Integrate patient and laboratory data**
   - Retrieve associated patient-level and laboratory data for selected metabolites to support translational and clinical insights.
3. **Identify interacting genes and proteins**
   - Discover genes and proteins interacting with a given biomarker to elucidate biological pathways, reactions, and post-translational modifications that may underlie disease processes.



