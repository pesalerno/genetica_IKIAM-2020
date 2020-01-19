# genetica_IKIAM-2020
>Contenido de sección de introducción a la bioinformática del curso de Genetica para las Carreras de Ing. en Ecosistemas y de Ing. en Biotecnologia de la Universidad Regional Amazonica IKIAM, dictada durante el semestre Oct 2019 - Feb 2020 por la Dra. Patricia E. Salerno.

**1. Introducción a repositorios git y su importancia.**
----



**2. Comenzando con datos de Illumina.** 
----


>Los siguientes datos están basados en un proyecto de filogenómica de ranas venenosas de Ecuador y Colombia, y en este caso tenemos secuencias de doce individuos que abarcan tres géneros y 20 millones de años de evolución. Este taller de intro a bioinfo de genómica está [basado en otro taller](https://rdtarvin.github.io/IBS2019_Genomics-of-Biodiversity/) que Rebecca Tarvin y yo diseñamos e impartimos recientemente en la Universidad San Francisco de Quito, y [ésta parte en específico del taller](https://rdtarvin.github.io/IBS2019_Genomics-of-Biodiversity/main/2019/08/05/02-shell.html) la escribió Rebecca usando una pequeña muestra semi-aleatoria de los datos para un proyecto en el que ella trabaja de hace muchos años! Rebecca hoy es [profesora titular de la Universidad de California en Berkeley](https://ib.berkeley.edu/people/faculty/tarvinr).  


Primero, abre el shell y crea un directorio para bioinformática para el curso, y luego dentro de ese directorio, crea otro subdirectorio llamado `1ra-practica` donde bajaremos utilizando el comando `curl` los datos de Illumina que usaremos para el curso. 

 	mkdir BIOINFO-genetica
	cd BIOINFO-genetica/
	mkdir 1ra-pratica
	cd 1ra-pratica/
 	curl -L -O https://www.dropbox.com/s/nuxs8c3po3mqksa/epiddrad_5M_R1_.fastq.gz?dl=1
 	curl -L -O https://www.dropbox.com/s/szf7lud0yhf7rqo/epiddrad_5M_R2_.fastq.gz?dl=1
 	
 Ahora veamos cómo se ven nuestros datos. Hagamos una lista de nuestros archivos en el directorio en el que estamos en este momento, y cambiemos los nombres a algo mas sencillo y que tengan la terminación correcta. 
 
 	ls
 	ls -lhtr 
 	# -l = long format, -h = human readable, -t = order by time -r = time reversed
 	
 	mv epiddrad_5M_R1_.fastq.gz\?dl\=1 epiddrad_5M_R1.fastq.gz
 	##hacerlo igual para el otro archivo
 	
 	ls 
 	#ahora ambos nombres deberian tener la terminación correcta de archivo y el nombre más sencillo. 
 
 
 Ahora, nos damos cuenta que es mejor que organicemos nuestros datos crudos en una carpeta/directorio separado, asi que creamos un nuevo directorio llamado `datos-crudos` y usamos el ***wildcard*** `*` para mover ambos archivos usando un único comando. 
 
 	mkdir datos-crudos
 	mv *.fq.gz datos-crudos
 	cd datos-crudos
 	
 Ahora, comencemos a ver los archivos que nos mandaron de Illumina. Intentemos verlo de un par de formas con el archivo comprimido utilizando el programa `less`. 
 
 	less epiddrad_5M_R1.fastq.gz
 	#para salir, poner control X
 	
 	zless epiddrad_5M_R1.fastq.gz
 
 Ahora podemos ver lo que es el formato fastc de Illumina, quien nos manda el consenso del cluster de clones de secuencias y la calidad asociada a cada base secuenciada para cada una de nuestras millones de secuencias. 
 
 	@K00179:78:HJ2KFBBXX:5:2121:8907:46786 1:N:0:TGACCA+AGATCT
	GCATGCATGCATATGATGTCACTGTAATGCGCCGGCCCATCAGAATCTCATATGAATCTAATATGGGACCAGGATGGAGACATATGGGATCCAGGAAGGGAGAAAAGATTACCTAAAGGGGCTAGGAACACAATACTAATGTATGAGAAA
	+
	AAFFFJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ

Ahora, descomprimimos el archivo para moder manipularlo mas con otros comandos, asi como `head` y `tail` que nos ayudan a ver comienzo y fin de un archivo de texto, `cat` el cual imprime/visualiza archivos de texto en su totalidad, y `grep` que nos ayuda a contar lineas. 

	gunzip *.gz
	ls #se terminaron de descomprimir?
	
	cat epiddrad_5M_R1.fastq.gz
 	#y ahora?? :-O
 	
 Este es el problema principal con cualquier dato de secuenciación masiva, que simplemente los datos crudos no son visualizables, porque son demasiado grandes. Entonces, tenemos que observar los datos de a poco y con ciertos programas usando la línea de comando. 
 
 	# print the first 10 lines of a file
	head epiddrad_5M_R1_.fastq

	# print the first 20 lines of a file
	head epiddrad_5M_R1_.fastq -20 # '-20' is an argument for the 'head' program

	# print lines 190-200 of a file
	head -200 epiddrad_5M_R1_.fastq | tail # 'pipes' the first 200 lines to a program called tail, which prints the last 10 lines

	# view the file interactively
	less epiddrad_5M_R1_.fastq # can scroll through file with cursor, page with spacebar; quit with 'q'
	# NOTE: here we can use less because the file is not in gzipped (remember that required the 'zless' command)

	# open up manual for less program
	man less # press q to quit

	# print only the first 10 lines and only the first 30 characters of each line
	head -200 epiddrad_5M_R1_.fastq | cut -c -30 

 
 A parte de siplemente visualizar nuestro archivo con ciertos comandos, podemos crear programas/pipelines sencillos de formato `standard_in>standard_out` para contar numeros de secuencias en total o por determinado **barcode** de interés dentro de nuestro *pool* de muestras secuenciadas. 
 
 	# count the number of lines in the file
	wc -l epiddrad_5M_R1_.fastq # (this takes a moment because the file is large)

	# print lines with AAAAA in them
	grep 'AAAAA' epiddrad_5M_R1_.fastq # ctrl+c to exit

	# count lines with AAAAA in them
	grep -c 'AAAAA' epiddrad_5M_R1_.fastq

	# save lines with AAAAA in them as a separate file
	grep 'AAAAA' epiddrad_5M_R1_.fastq > AAAAA # no file extensions necessary!!

	# add lines with TTTTT to the AAAAA file: '>' writes or overwrites file; '>>' writes or appends to file
	grep 'TTTTT' epiddrad_5M_R1_.fastq >> AAAAA 

	# print lines with aaaaa in them
	grep 'aaaaa' epiddrad_5M_R1_.fastq
	# why doesn't this produce any output?

	# count number of uniq sequences in file with pattern 'AGAT'
	grep 'AGAT' epiddrad_5M_R1_.fastq | sort | uniq | wc -l

	# print only the second field of the sequence information line
	head epiddrad_5M_R1_.fastq | grep '@' | awk -F':' '{ print $2 }' 
	# awk is a very useful program for parsing files; here ':' is the delimiter, $2 is the column location
 	
