# genetica_IKIAM-2020
>Contenido de sección de introducción a la bioinformática del curso de Genetica para las Carreras de Ing. en Ecosistemas y de Ing. en Biotecnologia de la Universidad Regional Amazonica IKIAM, dictada durante el semestre Oct 2019 - Feb 2020 por la Dra. Patricia E. Salerno.

**1. Introducción a repositorios git y su importancia.**
----
Github es un software en línea y local que ayuda a colaborar en proyectos bioinformáticos y más en general de programación. 

- open source & gratis
- version control
- repositorios colaborativos
- repositorios publicos o privados

>Instala [github para ubuntu aca](https://www.liquidweb.com/kb/install-git-ubuntu-16-04-lts/), para que en la siguiente clase intentemos hacer nuestro primer repositorio. 

Por ahora, hagamos el ejercicio más básico de uso de github: bajarse el archivo llamado **README.md** del repositorio del curso llamado `genetica_IKIAM-2020`. Si quieren se bajan el repositorio completo, o van directo al archivo y se bajan unicamente ese. Lo lograron? 

Intenten abrir el archivo **README.md** en formato *raw* en línea... que características generales tiene un archivo de formato [Markdown](http://pad.haroopress.com/user.html) (terminación `.md`)? 


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
 	mv *.fastq.gz datos-crudos
 	cd datos-crudos
 	
 Ahora, comencemos a ver los archivos que nos mandaron de Illumina. Intentemos verlo de un par de formas con el archivo comprimido utilizando el programa `less`. 
 
 	less epiddrad_5M_R1.fastq.gz
 	#para salir, poner control X
 	
 	zless epiddrad_5M_R1.fastq.gz
 
 Ahora podemos ver lo que es el formato fastq de Illumina, quien nos manda el consenso del cluster de clones de secuencias y la calidad asociada a cada base secuenciada para cada una de nuestras millones de secuencias. 
 
 	@K00179:78:HJ2KFBBXX:5:2121:8907:46786 1:N:0:TGACCA+AGATCT
	GCATGCATGCATATGATGTCACTGTAATGCGCCGGCCCATCAGAATCTCATATGAATCTAATATGGGACCAGGATGGAGACATATGGGATCCAGGAAGGGAGAAAAGATTACCTAAAGGGGCTAGGAACACAATACTAATGTATGAGAAA
	+
	AAFFFJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ

>Para recordar cómo funciona la química de la secuenciación Illumina vea [este video](https://www.youtube.com/watch?v=fCd6B5HRaZ8). 

Ahora, descomprimimos el archivo para moder manipularlo más con otros comandos, asi como `head` y `tail` que nos ayudan a ver comienzo y fin de un archivo de texto respectivamente, `cat` el cual imprime/visualiza archivos de texto en su totalidad, y `grep` que nos ayuda a contar líneas y caracteres, y extraerlos a otro output. 

	gunzip *.gz
	ls #se terminaron de descomprimir?
	
	cat epiddrad_5M_R1.fastq
 	#y ahora?? :-O
 	
 Este es el problema principal con cualquier dato de secuenciación masiva, que simplemente los datos crudos no son visualizables, porque son demasiado grandes. Entonces, tenemos que observar los datos de a poco y con ciertos programas usando la línea de comando. Primero, intentemos el comando más simple:
 
 	head epiddrad_5M_R1.fastq
 	tail epiddrad_5M_R1.fastq
 
 Para imprimir las primeras 20 líneas de un archivo:
 
	head -20 epiddrad_5M_R1.fastq 

Donde '-20' es un argumento para el programa `head`. Para imprimir entre las líneas 190 y 200, se puede escribir un programa sencillo utilizando `head` y `tail` junto con `|` (o mejor conocido como *'pipe'*). 

	head -200 epiddrad_5M_R1.fastq | tail 
	
	# 'pipes' the first 200 lines to a program called tail, which prints the last 10 lines


También podemos visualizar el archivo por partes utilizando `less`: 

	less epiddrad_5M_R1.fastq 
	
	# can scroll through file with cursor, page with spacebar; quit with 'q'
	

>Si desean ver el manual para el programa, pueden escribir: 
>
	man less # press q to quit

 
 Podemos usar el programa `wc`, o *word count* para contar el numero de líneas, por ejemplo, en un archivo. 
 
 	wc -l epiddrad_5M_R1.fastq 
 		
 Pero, esto no parece muy útil dado que no todas las líneas son secuencias... aunque podemos usar una simple division (total lineas dividido entre cuatro nos daria el numero de secuencias).... En todo caso, ahora lo que queremos es contar el número de secuencias por un determinado barcode de secuencia conocida, y guardarlas luego en otro archivo, por lo que usamos: 
 
 	grep -c 'AAAAA' epiddrad_5M_R1.fastq
 	grep 'AAAAA' epiddrad_5M_R1.fastq > barcode.txt
 	

También puede que queramos agregar otro barcode a ese mismo archivo, por lo que usamos `>>` para comenzar a escribir al final del archivo preexistente - en vez de reescribir!

	grep 'TTTTT' epiddrad_5M_R1.fastq >> AAAAA 



> Otros ejercicios: 
> 
> - ejercicio usando `|` y `cut`:
>
	head -200 epiddrad\_5M_R1.fastq | cut -c -30 
	
>	---> que hace esta linea de codigo? 
>	
> - contar el número de secuencias únicas en el archivo y que contengan la secuencia **'AGAT'** .... cuántos programas se usaron en total?? 
> 
	grep 'AGAT' epiddrad\_5M_R1.fastq | sort | uniq | wc -l


 	
 **3. Visualizando calidad de corrida de Illumina.** 
----
Primero, bajemos e instalemos el programa [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), y guardemoslo dentro de un directorio llamado `programas` dentro del directorio `BIOINFO-genetica`. Este programa se ha vuelto un estándard de informe de calidad de una corrida de secuenciación de "high-throughput", así como lo es Illumina. 

	mkdir programas
	cd programas

Luego, para instalar el programa utilicemos la línea de comando y el programa `curl` de la siguiente manera:
	
	curl -L -O https://www.bioinformatics.babraham.ac.uk/projects/	fastqc/fastqc_v0.11.5.zip
	
Dentro de su carpeta de `programas`, descomprima el archivo descargado que debe tener un nombre parecido a `fastqc_v0.11.5.zip`. Una vez descomprimido, está listo para ser usado. Para correr el programa:

	relative/path/to/programs/FastQC/fastqc *.fastq

	open epiddrad_5M_R1_fastqc.html