# Requisitos del sistema

## Sistema operativo
Ubuntu/Linux
## Crear directorio de trabajo

```bash
mkdir tuberculosis
cd tuberculosis
```

---

## PARTE 1 - Descargar dataset FASTQ
*datasets download genome accession GCF_000195955.2
Del NCBI SRA (Sequence Read Archive) descargar: 
+SRR38304207  
-SRR38388669  
*SRR26387480  
+SRR36403484  
-SRR38510712  

Descomprimir archivo de referencia 
```bash
unzip ncbi dataset
```

---  
# PARTE 2 — Control de calidad con FastQC
## Ejecutar FastQC
Evaluaciòn de control de calidad de mùltiples archivos fastq.gz a la vez
```bash
fastqc *.fastq.gz
```

---  

## Archivos generados

```bash
ls
```
---  

# PARTE 3 — Trimming con Trimmomatic  

```bash
for file in *.fastq.gz; do 
output="${file%.fastq.gz}_trimmed.fastq.gz" 
echo "Procesando archivo: $file ..." 
java -jar /usr/share/java/trimmomatic-0.39.jar SE -phred33 \ 
"$file" "$output" \ 
HEADCROP:15 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:20 MINLEN:36 \ 
2>&1 | tee "${file}_trimming.log" 
echo "Finalizado: $output" 
done
```
---  

# PARTE 4 Control de calidad con FastQC en secuencias trimadas  

```bash
fastqc -t 4 *_trimmed.fastq.gz
```
---   

# PARTE 5 Reporte de MultiQC antes y despues de Trimmomatic
```bash
multiqc /home/lauraperez/tuberculosis/reportes_calidad/ --ignore "*trimmed*" -n multiqc_CRUDOS.html -f -t simple --flat
```
```bash
multiqc /home/lauraperez/tuberculosis/reportes_calidad/ -f -t simple --flat
```
---

# PARTE 6 Alineamiento de lecturas  
# Reubicaciòn de genoma de referencia y cambio de nombre (opcional)
Mover archivo de genoma de referencia H37Rv a carpeta principal tuberculosis y cambio de nombre por referencia.fna 
```bash
$ cp ncbi_dataset/data/GCF_000195955.2/GCF_000195955.2_ASM19595v2_ 
genomic.fna ~/tuberculosis/referencia.fna
```

# Indexar referencia
```bash
wa index referencia.fna 
```

# Alinear lecturas con genoma de referencia
```bash
for file in *_trimmed.fastq.gz; do 
sample="${file%_trimmed.fastq.gz}" 
echo "Alineando muestra: $sample" 
bwa mem -t 4 referencia.fna "$file" > "${sample}.sam" 
done
```
---

# PARTE 7 Conversiòn SAM a BAM
```bash
for samfile in *.sam; do
    sample="${samfile%.sam}";
    echo "Procesando $sample: SAM -> BAM ordenado";
    samtools view -S -b "$samfile" | samtools sort -o "${sample}_sorted.bam";
    samtools index "${sample}_sorted.bam";
done
```

# Limpieza de archivos temporales y pesados
```bash
rm *.tmp.*.bam
rm *.sam
```
---

# PARTE 8 Llamado de variantes
```bash
for bamfile in *_sorted.bam; do
    sample="${bamfile%_sorted.bam}";
    echo "Analizando mutaciones en: $sample";
```

# Generación de variantes crudas (BCF)
```bash
    bcftools mpileup -f referencia.fna "$bamfile" | bcftools call -mv -Ob -o "${sample}.bcf";
```

# Filtrado por calidad (QUAL > 30) y profundidad (DP > 10)
```bash
bcftools view -i 'QUAL>30 && DP>10' "${sample}.bcf" -Oz -o "${sample}_final.vcf.gz";
```

# Indexación del VCF final y limpieza de BCF temporal
```bash
bcftools index "${sample}_final.vcf.gz";
    rm "${sample}.bcf";
done
```
---

# PARTE 9 Consolidaciòn de variantes y matriz genòmica

# Unión de todos los archivos VCF en una matriz única e indexar
```bash
bcftools merge *_final.vcf.gz -Oz -o matriz_tuberculosis.vcf.gz
bcftools index matriz_tuberculosis.vcf.gz
```
# Filtrado selectivo de SNPs (eliminación de Indels y sitios invariantes)
```bash
bcftools view -v snps matriz_tuberculosis.vcf.gz -Oz -o matriz_solo_snps.vcf.gz
bcftools index matriz_solo_snps.vcf.gz
```
# Verificación del número de variantes identificadas
```bash
bcftools view -H matriz_solo_snps.vcf.gz | wc -l
```
# Verificación de muestras y conversión de formatos para interoperabilidad
```bash
bcftools query -l matriz_solo_snps.vcf.gz
gunzip -c matriz_solo_snps.vcf.gz > matriz_final.vcf
snp-sites -p -o matriz_snps.phy matriz_final.vcf
```
# PARTE 10 Generación del Alineamiento de Consenso con Referencia

# Inclusión manual de la Referencia H37Rv en el archivo final
```bash
echo ">Referencia_H37Rv" > alineamiento_con_ref.fasta
grep -v ">" referencia.fna | tr -d '\n' >> alineamiento_con_ref.fasta
echo "" >> alineamiento_con_ref.fasta
```

# Generación de secuencias consenso para cada muestra geográfica
```bash
for sample in $(bcftools query -l matriz_solo_snps.vcf.gz); do 
    echo ">$sample" >> alineamiento_con_ref.fasta; 
    bcftools consensus -f referencia.fna -s "$sample" matriz_solo_snps.vcf.gz | grep -v ">" | tr -d '\n' >> alineamiento_con_ref.fasta; 
    echo "" >> alineamiento_con_ref.fasta; 
done
```

# PARTE 10 Elaboraciòn àrbol filogenètico final
# Extracción de la matriz de SNPs definitiva incluyendo la referencia
```bash
snp-sites -o matriz_con_referencia.fasta alineamiento_con_ref.fasta
```
# Inferencia del árbol por Máxima Verosimilitud (IQ-TREE 2)
```bash
iqtree2 -s matriz_con_referencia.fasta -m GTR+G -bb 1000 -nt AUTO -pre arbol_con_ref
```
