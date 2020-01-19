# genetica_IKIAM-2020
>Contenido de sección de bioinformática del curso de Genetica para las Carreras de Ing. en Ecosistemas y de Ing. en Biotecnologia de la Universidad Regional Amazonica IKIAM, dictada durante el semestre Oct 2019 - Feb 2020 por la Dra. Patricia E. Salerno.

1. Introducción a repositorios git y su importancia.
----



2. Comenzando con datos de Illumina. 
----


>Los siguientes datos están basados en un proyecto de filogenómica de ranas venenosas de Ecuador y Colombia, y en este caso tenemos secuencias de doce individuos que abarcan tres géneros y 20 millones de años de evolución. Este taller de intro a bioinfo de genómica está [basado en otro taller](https://rdtarvin.github.io/IBS2019_Genomics-of-Biodiversity/) que Rebecca Tarvin y yo diseñamos e impartimos recientemente en la Universidad San Francisco de Quito, y [ésta parte en específico del taller](https://rdtarvin.github.io/IBS2019_Genomics-of-Biodiversity/main/2019/08/05/02-shell.html) la escribió Rebecca usando una pequeña muestra semi-aleatoria de los datos para un proyecto en el que ella trabaja de hace muchos años! Rebecca hoy es [profesora titular de la Universidad de California en Berkeley](https://ib.berkeley.edu/people/faculty/tarvinr).  


Primero, abre el shell y crea un directorio para bioinformática para el curso, y luego dentro de ese directorio, crea otro subdirectorio `1ra-practica` donde bajaremos los datos de Illumina que usaremos para el curso. 

 	mkdir BIOINFO-genetica
	cd BIOINFO-genetica/
	mkdir 1ra-pratica
	cd 1ra-pratica/
 	curl -L -O https://www.dropbox.com/s/nuxs8c3po3mqksa/epiddrad_5M_R1_.fastq.gz?dl=1
 	curl -L -O https://www.dropbox.com/s/szf7lud0yhf7rqo/epiddrad_5M_R2_.fastq.gz?dl=1
 	
 Ahora veamos cómo se ven nuestros datos. Hagamos una lista de nuestros archivos en el directorio en el que estamos en este momento, y cambiemos los nombres a algo mas sencillo y que tengan la terminación correcta. 
 
 	ls
 	ls -ltr
 	mv epiddrad_5M_R1_.fastq.gz\?dl\=1 epiddrad_5M_R1.fastq.gz
 	##hacerlo igual para el otro archivo
 	ls #ahora ambos nombres deberian tener la terminacon correcta de archivo y el nombre mas sencillo. 
 
 
