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
   - Generated files are available in:  
     ```
     /data/shared/repos/ubkg-etl
     ```

2. **Integration into CFDE KG**
   - Once generated, these files are re-ingested into the CFDE KG.  
   - Full build and integration instructions are provided by the UBKG team:  
     [UBKG Build Instructions](https://github.com/x-atlas-consortia/ubkg-neo4j/blob/main/docs/BUILD_INSTRUCTIONS.md).  

3. **Data Updates**
   - Each time new biomarker data is pushed, the edge list and node files must be **recreated and re-ingested** into the CFDE KG.
  
## Notes & Requirements  

- Building the KG requires significant disk space:  
  > **Free disk space equal to 3–4× the size of the ontology CSVs is recommended.**

- Repository copies and scripts are stored here:  

