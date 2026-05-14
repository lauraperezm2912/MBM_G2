# Proyecto: Metagenomic DNA sequencing to quantify *Mycobacterium tuberculosis* DNA and diagnose tuberculosis  
## Integrantes  
*Pérez Laura  
*Mindiola Edwar  
*Baquedano Genesis  
*Guzman Genesis  

## Objetivo
Realizar un análisis comparativo de variantes genómicas en muestras de *Mycobacterium tuberculosis* obtenidas de cohortes geográficamente diversas (Uganda, Rusia ,India, Argentina y EE. UU.), con el fin de reconstruir su historia evolutiva mediante filogenia.  

## Dataset
datasets download genome accession GCF_000195955.2 --include gff3,rna,cds,protein,genome,seq-report (genome reference)  
Datasets de pacientes ubicados en diversos países y regiones, incluyendo Uganda, Argentina y la India, así como en ciudades y estados específicos como Moscú, San Petersburgo y Texas.

Paciente ubicación Uganda: SRR38304207  
Paciente ubicación Moscú: SRR38388669  
Paciente ubicación San Petersburgo: SRR26387480  
Paciente ubicación India: SRR36403484  
Paciente ubicación Argentina: SRR38405735  

## Flujo de Trabajo
```mermaid
graph TB
    %% Definición de Estilos (Tus colores originales)
    classDef input fill:#fdf2f2,stroke:#f05252,stroke-width:2px,color:#000;
    classDef process fill:#e1effe,stroke:#3f83f8,stroke-width:2px,color:#000;
    classDef reference fill:#f3f4f6,stroke:#4b5563,stroke-width:2px,color:#000;
    classDef output fill:#f0fdf4,stroke:#22c55e,stroke-width:2px,color:#000,stroke-dasharray: 5 5;

    %% Nivel 1: Entrada
    Raw[<b>Entrada de datos</b><br/> NCBI: Lecturas crudas FASTQ<br/>Uganda, Rusia, India, Argentina, USA]:::input

    %% Nivel 2: Calidad
    FQC(<b>Control de Calidad</b><br/>FastQC):::process
    Trim(<b>Limpieza y Trimado</b><br/>Trimmomatic<br/>HEADCROP:15<br/>SLIDINGWINDOW:4:20):::process

    %% Nivel 3: Alineamiento
    Ref[(<b>Referencia</b><br/><i>*M. tuberculosis*</i><br/>H37Rv.fna)]:::reference
    Map(<b>Alineamiento</b><br/>BWA-MEM):::process

    %% Nivel 4: Post-procesamiento
    SAM(<b>Samtools</b><br/> Conversión SAM a BAM<br/>Ordenar e Indexar):::process
    VCF(<b>Variantes</b><br/>BCFtools:<br/>Detección de SNPs):::process

    %% Nivel 5: Resultado
    Phylo(<b>Filogenia</b><br/>IQ-TREE<br/>GTR+G):::process
    Tree{{<b>Salida Final</b><br/>Árbol Evolutivo<br/> Visualización: iTOL / FigTree}}:::output

    %% Flujo Vertical
    Raw --> FQC
    FQC --> Trim
    Trim --> Map
    Ref --> Map
    Map --> SAM
    SAM --> VCF
    VCF --> Phylo
    Phylo --> Tree  
   
```

```mermaid
graph TD
    %% Definición de Estilos
    classDef input fill:#fdf2f2,stroke:#f05252,stroke-width:2px,color:#000;
    classDef process fill:#e1effe,stroke:#3f83f8,stroke-width:2px,color:#000;
    classDef reference fill:#f3f4f6,stroke:#4b5563,stroke-width:2px,color:#000;
    classDef output fill:#f0fdf4,stroke:#22c55e,stroke-width:2px,color:#000,stroke-dasharray: 5 5;

    %% Primera fila: Preparación
    Raw[<b>Entrada de datos</b><br/>NCBI: FASTQ]:::input --> FQC(<b>Calidad</b><br/>FastQC):::process
    FQC --> Trim(<b>Limpieza</b><br/>Trimmomatic):::process
    
    %% Conexión hacia abajo y cambio de dirección
    direction LR
    Trim --> Map(<b>Mapeo</b><br/>BWA-MEM):::process
    Ref[(<b>Referencia</b><br/>H37Rv)]:::reference --> Map

    %% Segunda fila: Análisis
    direction TB
    Map --> SAM(<b>Samtools</b><br/>SAM/BAM):::process
    SAM --> VCF(<b>Variantes</b><br/>BCFtools SNPs):::process

    %% Tercera fila: Resultados final
    direction RL
    VCF --> Phylo(<b>Filogenia</b><br/>IQ-TREE):::process
    Phylo --> Tree{{<b>Resultado</b><br/>Árbol Evolutivo}}:::output

```

## Resultados  
## Bibliografía
Doughty, E. L., Sergeant, M. J., Adetifa, I., Antonio, M., Pallen, M. J., & Clark, T. G. (2022). *Metagenomic DNA sequencing to quantify Mycobacterium tuberculosis DNA and diagnose tuberculosis*. Scientific Reports, 12, 17937. https://doi.org/10.1038/s41598-022-21244-x 
## Contribucion individual
##  Cómo reproducir (scripts)
