# TALLER | Filtrado y Genómica Poblacional

>Este taller debe ser entregado en pareja para el jueves 13 de Febrero a mas tardar 11pm como parte del **examen parcial II** de genética. 
>
NOMBRES | APELLIDOS
-------------|--------------|
nombrescompletos1 | apellidoscompletoss1|
nombrescompletos2 | apellidoscompleto2|
		

## 1. Instalando **vcftools** y bajando la matriz de SNPs

Primero, debemos hacer un par de cosas para que el programa a utilizar, `vcftools`, quede correctamente compilado e instalado. 

Instalemos una [version vieja de **vcftools**](https://sourceforge.net/projects/vcftools/files/vcftools_0.1.13.tar.gz/download), el cual se debe bajar automaticamente haciendo click en el link anterior, y luego lo descomprimimos utilizando el siguiente codigo: 

	tar -xvf vcftools.version

Luego hacemos `cd` al directorio nuevo de vcftools, y escribimos: 

	./configure ##puede que este paso no sea necesario, vean primero si tienen el archivo "makefile"
	make
	make install

Si todo sale bien, deberiamos ya tener **vcftools** instalado en ese mismo directorio, el cual pueden revisar que funcione bien escribiendo: 

	./vcftools 

Luego, obtenemos nuestra [matriz de datos de este link]() y ponemos ese archivo (con terminacion `.vcf`) dentro de la misma carpeta donde esta vcftools. Para saber el directorio exacto de donde esta vcftools, hagan el comando `pwd` y pueden copiar el directorio completo para luego mover el archivo desde los downloads hasta alli. 


______________

## 2. Corriendo los filtros básicos en plink. 

Primero, queremos asegurarnos de no tener un monton de SNPs que no tienen nada de información y roban mucho tiempo de computación, por lo que eliminamos los SNPs que estan presentes solo en pocos individuos: 

	./vcftools --vcf populations.snps.vcf --max-missing 0.5 --recode --out filtered-snps-1

>-------------------------
>**PREGUNTA 1: cuántos loci se retienen luego de este primer filtro:**
>
>**RESPUESTA:** 
>
>---------------------------
>
>**PREGUNTA 2: que exactamente estamos haciendo con el comando `--max-missing` de `vcftools`?**
>
>**RESPUESTA:** 
>
>------------------------
>

Segundo, queremos asegurarnos de no retener SNPs poco informativos y con alto potencial de ser simple reflejo de error, por lo que filtramos SNPs que tengan frecuencia alelica menor al 0.02: 

	./vcftools --vcf filtered-snps-1.vcf --maf 0.02 --recode --out filtered-snps-2
	
>---------------------
>**PREGUNTA 3: Cuantos loci se retienen luego de este filtro?** 
>
>**RESPUESTA:** 
>
>---------------------------

Ahora, queremos ver si algo cambia si en vez de utilizar frecuencia alelica usamos simplemtente "cuenta" alélica, es decir, veces totales que se cuenta el alelo menor (*sin importar tamaño de la muestra*): 

	./vcftools --vcf filtered-snps-1.recode.vcf --mac 1 --recode --out filtered-snps-2

>---------------------
>**PREGUNTA 4: Cuántos loci se retienen luego de este filtro?** 
>
>**RESPUESTA:** 
>
>---------------------------
>**PREGUNTA 5: Qué pasa si aumentamos a conteo alélico de 2?** 
>
>**RESPUESTA:** 
>
>---------------------------

Quedémonos con la última matriz generada, obtenida con el filtro `--mac 2` para asegurarnos que no tenemos nada que sea cerca de cero informativo para inferencias estadísticas, ya que tenemos bastantes SNPs retenidos.

---------------------

## 3. Estimando y filtrando Linkage Disequilibrium

Ahora, queremos estimar  