# Example Cypher Queries
Look for any node n that contains a string and return 5 of them
```
MATCH (n)
WHERE ANY(key IN keys(n) WHERE toString(n[key]) CONTAINS "PYGM")
RETURN n
LIMIT 5
```
Call for node relationships (there is a huge number, only return 100)
```
MATCH (startNode)-[r:isa]->(endNode)
RETURN startNode, r, endNode
LIMIT 100
```
```
CALL db.relationshipTypes() YIELD relationshipType
RETURN relationshipType
MATCH (n:Term)
RETURN n LIMIT 100
```
```
MATCH (gene)-[r:gene_associated_with_disease]->(disease)
WITH gene, labels(gene) AS geneLabels, COUNT(r) AS relationshipCount
RETURN geneLabels, gene, relationshipCount
ORDER BY relationshipCount DESC
```
If you already know the concept node you're looking for
```
MATCH (n:Concept {CUI: 'C2825745'})-[:gene_associated_with_disease]->(connectedNode)
RETURN connectedNode
LIMIT 10
```
If you already know the relationship you're looking for
```
MATCH (concept:Concept {CUI: 'C2825745'})-[:gene_associated_with_disease]->(connectedNode),MATCH (conectedNode)-[:PREF_TERM]->(term:Term),RETURN connectedNode, term,LIMIT 10
```
We're currently doing queries on knowledge that we already know. In the future, we want an LLM where we would type a question, and it will make a cypher query for us.
# File Descriptions
## KVM2 `/data/shared/KG`
- Here `build_container.sh` doesn't work, Daniall put it and `container.cfg` there as backup.
- `csv`: these are files that build the knowledge graph - they also exist within the ubkg directory on KVM2.
- `distribution` are instructions for building the kg
- What we're interested in is `run_KG_connection`
- `run_KG_noBiomarker` is the version of the kg without biomarker data. We use it when we have new biomarker data. It is used to re-ingest biomarker data into the full kg (minus biomarker data).
KVM2 /data/shared/KG/run_KG_connection
- `DataDistillery03Jan2025.zip` is the zip file with everything
- we run `build_container.sh`
- `container.cfg` is the container config file. It has all the parameters for the container script. It tells you where to pull the docker image from. It has the neo4j password for GUI. It has the ports: defaults are 7474 and 7687, they were changed to 4000 and 4500 by Miguel but then Dacian updated something and they no longer work.
## Building the container locally
What you need:
- the zip file
- container.cfg
- docker desktop from docker.dom and run it

Start the container:
```
./build_container.sh
```
All the parameters were set by CFDE ubkg. Locally ports 7474 and 7687 work. If you want to change ports, comment them out in container.cfg. For faster loading, change the read/write mode to "read". Daniall only changed it to "read-write" because they wanted to change something with Jorge for the paper. Once the container is built (it's in its own directory), you can go one directory up (in a different terminal tab).

## Back to KVM2 `/data/shared/KG`
```
python -m venv env # Create env (I think it's better to download a pre-built virtual environment for speed)
source env/bin/activate # Activate the env
cd test_connection
```
Here are python scripts. Looking into `biomarker_id.py`:

When you're querying a knowledge graph, you need a username and password. Each script takes care of that. The script then queries the KG.

`disease_query.py`:

You see queries here with \n instead of what we saw in the cypher queries (literally on a new line).

## Let's do the same locally
```
source env/bin/activate
cd test_connection
python disease_query.py
```
It runs a query on genes associated with disease (not specific) and limits the output to 20 - otherwise it returns the entire graph.

In the output we see the CUI (concept ID), the preferred gene name ("APOE gene"), the preferred disease name ("Atherosclerosis") and label nodes. Cmd is hard to read, in the web interface you can expand nodes and you'll see label nodes, preferred term nodes, property nodes. This was the first line.

The second line shows the inverse relationship: starts from the disease and connects it to the APOE gene.

So we have 10 pairs of results (20 total).
