# Proyecto: Metagenomic DNA sequencing to quantify *Mycobacterium tuberculosis* DNA and diagnose tuberculosis  
## Integrantes  
* Pérez Laura  
+ Mindiola Edwar  
- Baquedano Genesis  
* Guzman Genesis  

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
El procesamiento bioinformático mediante Trimmomatic optimizó significativamente la integridad de los datos de las cinco muestras geográficas, reduciendo las tasas de error iniciales del 36% al 18% tras la remoción técnica de adaptadores y bases de baja calidad. Esta limpieza fue validada por la estabilidad del contenido de Guanina-Citosina entre el 64% y 66%, valor característico del genoma de M. tuberculosis, y por una calidad Phred superior a Q30 que garantiza una precisión en la identificación de nucleótidos mayor al 99.9%. Asimismo, el análisis del contenido de secuencias por base demostró la eliminación de ruidos y fluctuaciones posicionales, logrando proporciones constantes y paralelas que resultan en datos homogéneos y aptos para un alineamiento genómico robusto y confiable.

A nivel filogenético, el análisis de máxima verosimilitud reveló un clado sólido con un valor de bootstrap de 100 que agrupa a las muestras de San Petersburgo, Uganda y Texas, destacando a las de San Petersburgo y Texas como las más emparentadas al compartir un nodo común. Mientras que las muestras de Moscú e India presentaron una mayor incertidumbre estadística con un valor de soporte de 63, su cercanía al eje principal del árbol sugiere una mayor similitud genética con la cepa ancestral en comparación con la muestra de Uganda. Esta última presentó la rama horizontal más larga de la filogenia, lo que evidencia una mayor acumulación de variaciones genéticas y una divergencia evolutiva más pronunciada respecto al ancestro común del grupo estudiado.  

## Bibliografía
Doughty, E. L., Sergeant, M. J., Adetifa, I., Antonio, M., Pallen, M. J., & Clark, T. G. (2022). *Metagenomic DNA sequencing to quantify Mycobacterium tuberculosis DNA and diagnose tuberculosis*. Scientific Reports, 12, 17937. https://doi.org/10.1038/s41598-022-21244-x 


