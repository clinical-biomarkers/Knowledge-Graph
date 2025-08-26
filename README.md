# Knowledge-Graph

###Background

###KG Setup by Miguel:
Path where KG resdies within the KVM2 Server: data/shared/KG
- Contains all set up done by Miguel
- README.md gives all documentation on how to run the KG and LLM testing done so far
- distribution/ and csv/ are both set ups for building the BiomarkerKB KG
- https://github.com/clinical-biomarkers/biomarker-partnership/tree/main/supplementary_files 


###BiomarkerKB KG Build process
- The BiomarkerKB KG is built by ingesting biomarker data in the CFDE KG managed by the Data Distillery team
- https://ubkg.docs.xconsortia.org/basics/ 
Met with the team multiple times to get the process down
- First step was handled by Sean - Converted the Biomarker data in NTriples format to the OWLNETS_edgelist.txt and OWLNETS_node_metadata.txt files required by the data distillery knowledge graph.
https://github.com/clinical-biomarkers/nt-owlnets-kg-converter 
- Generated files exist here along with the entire repo copied from ubkg: /data/shared/repos/ubkg-etl
- Whenever we push new data the idea is to recreate the biomarker edge list and nodes and reingest the biomarker data into the CFDE KG
- https://github.com/x-atlas-consortia/ubkg-neo4j/blob/main/docs/BUILD_INSTRUCTIONS.md
- Important fact to note for the build: “Free disk space equal to 3-4 times the size of the set of ontology CSVs”
- Copied the repo here to get the relevant files and scripts: /data/shared/repos/ubkg-neo4j
