# TALLER | filtrando la matriz de SNPs

>Este taller debe ser entregado en pareja para el jueves 13 de Febrero a mas tardar 11pm como parte del **examen parcial II** de genética. 
>
**NOMBRES** | **APELLIDOS**
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


