# biopipeimport
**1. Project Name:** 
bioPipeImport v1.0.0

**2. Project repository:** 
https://github.com/umms-hackathon/biopipeimport

**3. Team members:**

**4. Aim:** 
This tool allows to import a standard nextflow pipeline.

**5. Project description:**
Overall import will support, parameters, channels, and process. 
Example nextflow is below. 


```
#!/usr/bin/env nextflow
 
/*
 * Defines pipeline parameters in order to specify the refence genomes
 * and read pairs by using the command line options
 */
params.reads = "$baseDir/data/ggal/*_{1,2}.fq"
params.genome = "$baseDir/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa"
   
/*
 * The reference genome file
 */
genome_file = file(params.genome)
  
/*
 * Creates the `read_pairs` channel that emits for each read-pair a tuple containing
 * three elements: the pair ID, the first read-pair file and the second read-pair file
 */
Channel
    .fromFilePairs( params.reads )                                             
    .ifEmpty { error "Cannot find any reads matching: ${params.reads}" }  
    .set { read_pairs }
  
/*
 * Step 1. Builds the genome index required by the mapping process
 */
process buildIndex {
    input:
    file genome from genome_file
      
    output:
    file 'genome.index*' into genome_index
        
    """
    bowtie2-build ${genome} genome.index
    """
}
``` 

After a successfull import. The pipeline will be visualized using d3.

An example workflow visualization.

![Alt text](img/example1.png?raw=true "Example Workflow Design")

**6. Supporting material and links:**
Supporting material. Sample files, external links.

Other examples can be found in the links below;

https://www.nextflow.io/example4.html


