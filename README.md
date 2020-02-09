# genetica_IKIAM-2020
>Contenido de sección de introducción a la bioinformática del curso de Genetica para las Carreras de Ing. en Ecosistemas y de Ing. en Biotecnologia de la Universidad Regional Amazonica IKIAM, dictada durante el semestre Oct 2019 - Feb 2020 por la Dra. Patricia E. Salerno.
>
>
>**NOTA IMPORTANTE:** Este taller esta diseñado para ser totalmente autoguiado. Todo el código de este repositorio - y en general del taller de bioinfo dictado en el curso de Genética - está diseñado para funcionar en el sistema operativo que se basa en unix... es decir, **NO funciona para PC** (sistema operativo DOS) incluso utilizando un programa de interface (asi como lo es cmder). Así que, a pesar de ser bastante autoguiado, si no utiliza un sistema operativo unix/linux/ubuntu/mac el código no servirá en su totalidad. 

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
 	
 Antes de empezar, comience por crear un directorio donde guardará todo lo que hagamos durante la práctica de hoy:
 	
 	mkdir 2da-practica
	cd 2da-practica	
 	
****
 >En general, comenzar a usar la línea de comando no es fácil. Pero, una vez que uno se acostumbra, luego resulta más fácil manejarse en el computador en línea de comando que usando el `GUI` del **buscador** o **finder**. Mientras se acostumbran a la línea de comando, deben apoyarse de recursos en línea para acostumbrarse a los comandos más usados, los cuales luego se les harán totalmente naturales. Por lo pronto, acá hay una [guia muy util](https://www.ntu.edu.sg/home/ehchua/programming/howto/Unix_SurvivalGuide.html) para comenzar, y abajo también les pongo una lista/compilación de comandos útiles y comunes: 
 
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
 
 Pero.... está tan lejos el directorio `Downloads` de donde estamos (es decir, el **path relativo** es muy largo) que tal vez es más fácil usar el **path absoluto**, que en este caso es: 
 
 	cd ~/Downloads
 
 >Se entendió la diferencia entre un *path* **relativo** y uno **absoluto**? Intenten navegar un poco por sus compus.... si se pierden, recuerden que siempre pueden acceder a su carpeta de `Documentos` desde el directorio `~`, por lo que siempre que se pierdan pueden escribir: 
> 
>	 	cd ~/Documents 
 
 
 
 
 
 **2. Entendiendo las librerías de ddRAD y los datos de Illumina.** 
----

****

>**NOTA:** recordando cómo funciona una libreria de **G**ENOTYPING **B**Y **S**EQUENCING (**GBS**) y entendiendo cómo fueron generadas las librerías de nuestros datos. 
>
>De muchas formas, las librerías de ddRAD se asemejan a otras técnicas moleculares como lo son los [RFLPs](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/RFLPs.png) y los [AFLPs](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/AFLPs.png). En las librerías de ddRAD, digerimos con dos [enzimas de restricción](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/restrictiondigest.png), las cuales dejan un **sticky end** de secuencia conocida y al cual *ligo* mis **adaptadores** con mis **barcodes únicos**. 
>
>
> En una librería de [GBS en general](https://github.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/blob/master/files/GBS-libraries.pdf), y en específico de [**ddRAD**](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/archivos/ddRAD-protocol.pdf) (protocolo con el cual se construyó la librería de [***Epipedobates***](https://en.wikipedia.org/wiki/Epipedobates)), se utilizan dos enzimas de restricción, en este caso **SphI + MluCl**, por lo que tenemos el siguiente formato de [nuestras librerías](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/RAD-vs-ddRAD.png) y de nuestros datos:
> 
>
![](https://github.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/blob/master/images/ddRAD-read.png?raw=true)
>
>el protocolo de ddRAD desde su publicación en el 2012 ha sido citado miles de veces debido a su versatilidad, ya que permite ajustarlo al tamaño del genoma del grupo de interés así como al porcentaje del genoma que quiere ser muestreado. 
>





********
## 3. Comenzando a desenredar los millones de datos utilizando el `pipeline` de **stacks** 


[Stacks](http://catchenlab.life.illinois.edu/stacks/manual/) es un 'cuasi-programa' el cual utiliza/une programas escritos en varios lenguajes como `C++`, `Perl`, `python`, y ayuda al usuario a generar un 'pipeline' de análisis que te lleva desde secuencias crudas hasta matrices de SNPs/secuencias concatenadas, y está especialmente diseñado para genotipificar loci generados de librerías de **GBS** - y más generalmente de librerías genómicas de representación reducida generadas con enzimas de digestión. 


Hoy, vamos a comenzar con el primer paso de cualquier librería de GBS, que es separar las secuencias en los individuos, lo cual hacemos a través de la secuencia única de barcode que fue asignada durante la preparación de las librerías. Recuerden que la razón principal por la cual RADseq/ddRAD son protocolos tan famosos es porque podemos hacer 'pooling' de decentas/cientos de individuos en [un mismo *'lane'*](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/pooling.png) de Illumina, ahorrándonos así miles de dólares y secuenciar una menor profundidad/porcentaje del genoma (ergo: secuenciación genómica de representación reducida).

>**MAS ACERCA DE NUESTRAS LIBRERIAS:** Luego de cortar nuestro genoma con las [enzimas de digestión](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/restrictiondigest.png) 'ligamos' (pegamos) los adaptadores a nuestros retazos del genoma, los cuales contienen los barcodes únicos para cada individuo - y esto lo debo llevar anotado en mi cuaderno de laboratorio!!! Pero de todas formas, yo ademas de tenerlo en mi cuaderno de lab lo tengo en el [siguiente archivo de texto](https://raw.githubusercontent.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/master/data/epi_barcodes.txt). Luego de que mis adaptadores están ligados... ya puedo unir todo el ADN genómico de mis muestras sin riesgo de contaminación! Por tanto, el primer paso en cualquier librería de representación reducida es el demultiplexing, o en otras palabras: 
>
>![](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/demultiplex.png)

Primero, comencemos con bajarnos [el siguiente archivo del drive](https://drive.google.com/open?id=1LBlEjBEKv1yePaKCau9NZ5_l59mRDoAl), y ponganlo dentro del directorio `2da-practica`. Ahora, intentemos ejecutar el archivo, el cual deberia ser un 'binario' que puede correr directamente en sistemas `unix` sin necesidad de ser compilado, utilizando el siguiente código (y desde el directorio donde está el binario ejecutable):

	./process_radtags 

...y ahora?? 

	

>**ACERCA DE archivos ejecutables y no ejecutables.** Si luego de intentar ejecutar cualquier archivo que deberia ser ejecutable (es decir, como cualquier `binario` de un programa), en la ventana del terminal sale: 
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
>vean nuevamente la info de los permisos.... [cómo cambió??](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/chmod.png).
>
>Ahora el archivo debe ser ejecutable sin ningún problema, y lo pueden probar usando el mismo código: 
>
> 		./process_radtags
> 

Ahora arreglemos el archivo de barcodes. Por los momentos, tenemos que nuestro archivo de 'barcodes' debe contener los barcodes en la columna de la izquierda y los nombres únicos (los cuales serán utilizados por el programa para darle los nombres a cada archivo del output) en la columna de la derecha... pero que vemos?! que nuestro archivo esta al reves.... ahora, en vez de abrir el archivo en excel (que creanlo o no tomaria mas pasos para arreglarlo debido al formato), abrimos el archivo en el programa **ATOM**, el cual es un EDITOR DE TEXTO SENCILLO. 

>**HABLEMOS DE ARCHIVOS DE TEXTO**. Los archivos de texto complejos como hechos en word o excel contienen millones de caracteres invisibles los cuales ayudan al formateo. Pero cuando editamos archivos de texto en informática, debemos hacerlo viendo TODOS los caracteres que están detrás. Para eso usamos ATOM, el cual nos permite visualizar caracteres invisibles. Si copiamos/pegamos el contenido de [esta pagina](https://raw.githubusercontent.com/rdtarvin/IBS2019_Genomics-of-Biodiversity/master/data/epi_barcodes.txt) en un archivo nuevo de ATOM, podemos hacer un 'find/replace' utilizando `grep` en ATOM para rápidamente modificar el archivo a lo que queremos. 
>
>![](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/grep.png)
>A este archivo modificado ponganle el nombre `barcodes.txt` y guárdenlo dentro del mismo directorio `2da-practica`. 

**NOTA:** como no tenemos editor de texto instalado en este momento en las compus de ubuntu, entonces [bajen de aca el archivo de barcodes](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/archivos/barcodes.txt) que usaremos para e paso siguiente. 

Ahora finalmente podemos correr la línea de comando para hacer el 'demultiplexing': 

	mkdir demultiplex
	./process_radtags -p ./datos-crudos -b ./barcodes.txt -o ./demultiplex -c -q -r -D --inline_null --renz_1 sphI --renz_2 mluCI

>Exactamente qué está sucediendo en este paso?? 

>![](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/fotos/stacks-1.png)

**************


# lunes 10 feb | cierre bioinfo y genómica


Al final de la práctica anterior hicimos el demultiplexing, o separación de corridas de Illumina de acuerdo al barcode de cada individuo. Después de este paso, estamos listos para comenzar con la genotipificación *denovo* utilizando el programa `stacks`. LA genotipificación *denovo* no es nada sencilla, por lo que nos saltamos este paso y vamos directo al filtrado despues de que ya tenemos una matriz de SNPs sin procesar - es decir, exportada sin nada de filtros desde `stacks`, para luego filtrarla en el programa `vcftools`.  


En este directorio puede ver los principales archivos de output de [demultiplexing](https://github.com/pesalerno/genetica_IKIAM-2020/tree/master/analisis/demultiplex), y en este otro directorio puede ver los archivos principales del [output de `denovo_map.pl` y de `populations`](https://github.com/pesalerno/genetica_IKIAM-2020/tree/master/analisis/denovo).  

-----------------------------------------
## TALLER | filtrando la matriz de SNPs con `vcftools`

>Para este taller estaremos usando otro [documento titulado **Taller de Filtrado y Genómica Poblacional**](https://github.com/pesalerno/genetica_IKIAM-2020/blob/master/TALLER_filtrando-SNPs.md). Deben entregar ese mismo documento con las respuestas en pareja para el **jueves 10pm**, preferiblemente (y con nota asociada) en formato markdown (`.md`) o en su defecto en formato `.pdf` exportado desde un editor de MarkDown. 
>
>Para tener todos los datos/archivos/documentos necesarios para la practica de hoy, haga click en el botón de `clone repository` dentro del repositorio principal del curso: 
>
>	[https://github.com/pesalerno/genetica_IKIAM-2020]()
>
>Alli, dentro de la carpeta llamada **analisis** encontraran todos los archivos necesarios para la práctica. En particular, debemos copiar el archivo `.vcf` al directorio local - en sus computadores - donde se instaló el programa - *y el ejecutable* - `vcftools`. 


