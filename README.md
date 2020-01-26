# genetica_IKIAM-2020
>Contenido de sección de introducción a la bioinformática del curso de Genetica para las Carreras de Ing. en Ecosistemas y de Ing. en Biotecnologia de la Universidad Regional Amazonica IKIAM, dictada durante el semestre Oct 2019 - Feb 2020 por la Dra. Patricia E. Salerno.
>
>
>**NOTA IMPORTANTE:** Este taller esta diseñado para ser totalmente autoguiado. Todo el código de este repositorio - y en general del taller de bioinfo dictado en el curso de Genética - está diseñado para funcionar en el sistema operativo que se basa en unix... es decir, **NO funciona para PC** (sistema operativo DOS) incluso utilizando un programa de interface (asi como lo es cmder). Así que, a pesar de ser bastante autoguidado, si no utiliza un sistema operativo unix/linux/ubuntu/mac el código no servirá en su totalidad. 

# lunes 20 ene | introducción a bioinfo parte 1

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


# lunes 27 ene | introducción a bioinfo parte II
 	
****
 En general, comenzar a usar la línea de comando no es fácil. Pero, una vez que uno se acostumbra, luego resulta más fácil manejarse en el computador en línea de comando que usando el `GUI` del **buscador** o **finder**. Mientras se acostumbran a la línea de comando, deben apoyarse de recursos en línea para acostumbrarse a los comandos más usados, los cuales luego se les harán totalmente naturales. Por lo pronto, acá hay una [guia muy util](https://www.ntu.edu.sg/home/ehchua/programming/howto/Unix_SurvivalGuide.html) para comenzar, y abajo también les pongo una lista/compilación de comandos útiles y comunes: 
 
 Command | Translation | Examples
--------|-------------|---------
`cd` | **c**hange **d**irectory | `cd /absolute/path/of/the/directory/` <br> Go to the home directory by typing simply  `cd` or `cd ~` <br> Go up (back) a directory by typing `cd ..`
`pwd` | **p**rint **w**orking **d**irectory | `pwd`
`mkdir` | **m**ake **dir**ectory | `mkdir newDirectory` creates newDirectory in your current directory <br> Make a directory one level up with `mkdir ../newDirectory`
`cp` | **c**o<b>p</b>y | `cp file.txt newfile.txt` (and file.txt will still exist!)
`mv` | **m**o<b>v</b>e | `mv file.txt newfile.txt` (but file.txt will *no longer* exist!)
`rm` | **r**e<b>m</b>ove | `rm file.txt` removes file.txt <br> `rm -r directoryname/` removes the directory and all files within
`ls` | **l**i<b>s</b>t | `ls *.txt` lists all .txt files in current directory <br> `ls -a` lists all files including hidden ones in the current directory <br> `ls -l` lists all files in current directory including file sizes and timestamps <br> `ls -lh` does the same but changes file size format to be **h**uman-readable <br> `ls ../` lists files in the directory above the current one
`man` | **man**ual | `man ls` opens the manual for command `ls` (use `q` to escape page)
`grep` | **g**lobal **r**egular <br> **e**xpression **p**arser |  `grep ">" seqs.fasta` pulls out all sequence names in a fasta file <br> `grep -c ">" seqs.fasta` counts the number of those sequences <br> 
`cat` | con<b>cat</b>enate | `cat seqs.fasta` prints the contents of seqs.fasta to the screen (ie stdout)
`head` | **head** | `head seqs.fasta` prints the first 10 lines of the file <br> `head -n 3 seqs.fasta` prints first 3 lines
`tail` | **tail** | `tail seqs.fasta` prints the last 10 lines of the file <br> `tail -n 3 seqs.fasta` prints last 3 lines
`wc` | **w**ord **c**ount | `wc filename.txt` shows the number of new lines, number of words, and number of characters <br> `wc -l filename.txt` shows only the number of new lines <br> `wc -c filename.txt` shows only the number of characters
`sort` | **sort** | `sort filename.txt` sorts file and prints output
`uniq` | **uniq**ue | `uniq -u filename.txt` shows only unique elements of a list <br> (must use sort command first to cluster repeats)

 

## 1. Entendiendo mejor los directorios y los *paths* en línea de comando

 
 Hasta ahora, hemos creado nuevos directorios (carpetas) utilizando `mkdir`, hemos entrado a ese directorio con el comando `cd`. Pero, es importante entender bien cómo escribir buenos `paths`, tanto relativos como absolutos. Hablemos de paths. En mi compu local, donde ahorita me encuentro trabajando en mi repositorio de github para este curso (es decir, en poner al día este documento que lees) escribo el comando `pwd`, el cual me da el **path absoluto**: 
 
 	/Users/patriciasalerno/Documents/IKIAM/semestre_oct-2019/GENETICA/BIOINFO/GIT/genetica_IKIAM-2020
 	
 Esto quiere decir que yo guardé (cloné) el repositorio de github llamado `genetica_IKIAM-2020` dentro de una carpeta que yo llamé `GIT` dentro de otra carpeta que llame `BIOINFO` adentro de **incluso** otra carpeta llamada `GENETICA` dentro de otra carpeta llamada.... etc., etc. O, en otras palabras, tengo un desastre de directorios, y mi computador personal se ve así: 
 
 ![fotito](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/mac-paths-pati.png)
 
 
 
 
Entonces, si yo quiero regresar a mi carpeta llamada `Downloads` tengo una de dos opciones. Primero, podemos navegarnos a través del **path relativo**, de la siguiente manera: 

	cd ../../../../../../../Downloads
 
 Pero.... está tan lejos el directorio `Downloads` de donde estamos (es decir, el **path relativo** es muy largo) que tal vez es más fácil usar el path absoluto en este caso: 
 
 	cd ~/Downloads
 
 >Se entendió la diferencia entre un *path* relativo y uno absoluto? Intenten navegar un poco por sus compus.... 
 
 
 
 **2. Visualizando calidad de corrida de Illumina.** 
----

****

>**NOTA:** recordando cómo funciona una libreria de **G**ENOTYPING **B**Y **S**EQUENCING (**GBS**). 
>
> Antes de continuar, veamos cómo es que funciona una librería de [GBS en general](https://github.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/blob/master/files/GBS-libraries.pdf) , y en específico cómo se construyó esta librería utilizando el protocolo de [**ddRAD**[(). 


Primero, bajemos e instalemos el programa [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), y guardemoslo dentro de un directorio llamado `programas` dentro del directorio `BIOINFO-genetica`. Este programa se ha vuelto un estándard de informe de calidad de una corrida de secuenciación de "high-throughput", así como lo es Illumina. 

	mkdir 2da-practica
	cd 2da-practica
	mkdir programas
	cd programas

Luego, para instalar el programa utilicemos la línea de comando y el programa `curl` de la siguiente manera:
	
	curl -L -O https://www.bioinformatics.babraham.ac.uk/projects/	fastqc/fastqc_v0.11.5.zip

*****

>**NOTA:** Puede que su computador no tenga la version de java necesaria para correr la visualización de la calidad de la corrida, por lo que tendría que bajarse la instalación en [esta pagina web](https://www.oracle.com/technetwork/java/javase/downloads/jdk13-downloads-5672538.html) y escogiendo (haciendo click) en el archivo que se llama `jda-13.0.2_linux-x64_bin.tax.gz`, y luego escribiendo en el directorio donde se lo bajo: 
>
>		tar xfvz jda-13.0.2_linux-x64_bin.tax.gz
>
>lo cual descomprimirá el archivo 
>
>		./configure
>		./make
>		./make install
>		
>y si ese ultimo comando no funciona, entonces utilice el siguiente comando: 
>
>		./sudo make install 
>
>lo cual le pide el password, el cual es `estudiante` - y recuerde, no se vera ningun texto cuando escribe **porque el password es secreto!!**
	
Dentro de su carpeta de `programas`, descomprima el archivo descargado que debe tener un nombre parecido a `fastqc_v0.11.5.zip`. Una vez descomprimido, está listo para ser usado. Para correr el programa, primero debe decifrar el directorio relativo del programa para asi poder correr el comando desde el directorio deonde están los datos. Una recomendación de una manera de hacerlo facil, es abrir una ventana del buscador como ésta: 



 ![fotito](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/directorio-path.png)


Y "jalar" el nombre del archivo completo hasta la ventana de terminal, lo que le da el `directorio absoluto` del archivo que quiere ejecutar. 

>**NOTA:** Hablemos un poco de archivos ejecutables y no ejecutables.Si luego de intentar ejecutar cualquier archivo que deberia se ejecutable (es decir, como cualquier `biniario` de un programa) si la ventana del terminal te dice: 
>
>		-bash: ./process_radtags: Permission denied
>
>eso quiere decir que tu computador no tiene el permiso para ejecutar ese archivo. Por tanto debemos usar `chmod` para modificar los permisos de ese archivo: 
>
>		ls -ltr 
>		##vean todo lo que dice en cuanto a permisos en la izquierda
>		chmod +rwx process_radtags
>		ls -ltr
>
>vean nuevamente la info de los permisos.... [cómo cambió??](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/chmod.png).Ahora el archivo debe ser ejecutable sin ningn problema. 



Una vez decifrado el path relativo desde donde tenemos guardada el archivo de las secuencias, corremos el programa fastqc para obtener los datos resumidos de calidad de la corrida de Illumina:


	/Users/patriciasalerno/Documents/IKIAM/semestre_oct-2019/GENETICA/BIOINFO/GIT/genetica_IKIAM-2020/FastQC/fastqc epiddrad_5M_R1.fastq.gz

	open epiddrad_5M_R1_fastqc.html
	
Sequencing quality scores, "Q", run from 20 to 40. In the fastq file, these are seen as ASCII characters. 
The values are log-scaled: 20 = 1/100 errors; 30 = 1/1000 errors. Anything below 20 is garbage and anything between 20 and 30 should be reviewed.
There appear to be errors in the kmer content, but really these are just showing where the barcodes and restriction enzyme sites are. 

> Now, let's take a short pause to look into how ddRAD libraries are constructed in this [mini-lecture](https://github.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/blob/master/files/GBS-libraries.pdf) to understand why our Illumina reads look like this:

![](https://github.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/blob/master/images/ddRAD-read.png?raw=true)

---