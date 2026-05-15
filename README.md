# Proyecto: Metagenomic DNA sequencing to quantify *Mycobacterium tuberculosis* DNA and diagnose tuberculosis  
## Integrantes  
*Pérez Laura  
*Mindiola Edwar  
*Baquedano Genesis  
*Guzman Genesis  

## Objetivo
Realizar un análisis comparativo de variantes genómicas en muestras de *Mycobacterium tuberculosis* obtenidas de cohortes geográficamente diversas (Uganda, Rusia ,India y EE. UU.), con el fin de reconstruir su historia evolutiva mediante filogenia.
## Dataset
datasets download genome accession GCF_000195955.2 --include gff3,rna,cds,protein,genome,seq-report (genome reference)
Datasets de pacientes ubicados en diversos países y regiones, incluyendo Uganda y la India, así como en ciudades y estados específicos como Moscú, San Petersburgo y Texas.

Paciente ubicación Uganda: SRR38304207  
Paciente ubicación Moscú: SRR38388669  
Paciente ubicación San Petersburgo: SRR26387480  
Paciente ubicación India: SRR36403484  
Paciente ubicación Texas: SRR38510712   

## Flujo de Trabajo  
El análisis bioinformático se inició con un riguroso control de calidad de las lecturas crudas en formato FASTQ mediante FastQC, seguido de un procesamiento masivo con Trimmomatic empleando el parámetro HEADCROP:15 para eliminar sesgos de composición en el inicio de las secuencias, además de filtros de calidad por ventana deslizante (SLIDINGWINDOW:4:20). Las lecturas filtradas se alinearon contra el genoma de referencia de Mycobacterium tuberculosis H37Rv utilizando el algoritmo BWA-MEM. Tras el procesamiento de los archivos de alineamiento con Samtools, se procedió a la identificación de polimorfismos de nucleótido único (SNPs) mediante BCFtools. Finalmente, a partir de la matriz de variantes obtenida, se realizó la reconstrucción filogenética por el método de Máxima Verosimilitud utilizando IQ-TREE con 1000 réplicas de bootstrap, permitiendo así la inferencia de las relaciones evolutivas y la estructura poblacional de las muestras provenientes de las cinco cohortes geográficas en estudio y la cepa de referencia. Por último, se visualizó el árbol evolutivo en MegaX  

## Resultados  
<img src="resultados/imagenes/MEGA_tree_imagen..png " alt="Árbol Filogenético Tuberculosis" width="100%">  

## Bibliografía
Doughty, E. L., Sergeant, M. J., Adetifa, I., Antonio, M., Pallen, M. J., & Clark, T. G. (2022). *Metagenomic DNA sequencing to quantify Mycobacterium tuberculosis DNA and diagnose tuberculosis*. Scientific Reports, 12, 17937. https://doi.org/10.1038/s41598-022-21244-x 


