comenzando con github
-


Aca esta el paso a paso para que logren montar su primer repositorio de git. 

**1.Registrarse/Instalar git [aqui](https://github.com/join)**

**2.ABRIR un repositorio en el website de git** de la siguiente manera (pero poniendole el nombre de repositorio que quieran... corto! sin espacios!):

![new repo](https://github.com/pesalerno/MingaGenomica2019/blob/master/fotos/newrepo.png)


**3.Montar archivos en el repositorio utilizando la linea de comando** 

Primero, revisemos que tenemos git instalado en la linea de comando, desde el terminal: 

	which git 

De estar instalado correctamente, el anterior comando les deberia imprimir en el terminal donde esta ubicado/instalado git: 

	/usr/bin/git 

Si esto no les sale, entonces deben instalarlo de nuevo usando este [link](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)


Ahora, antes de trabajar en el repositorio en el computador LOCAL, vamos  a *"clonar"* el repositorio que creamos en linea en nuestro computador local, utilizando el siguiente codigo: 

	git clone webaddresshere

el "web adress" es literalmente la direccion web de su repositorio que acaban de crear, asi que haganle click al repositorio que crearon (para entrar en el) y de ahi copian y pegan la direccion web. 

Ahora, deberian tener el repositorio guardado donde hicieron el comando `git clone` entonces desde ahi mismo metanse en la carpeta que clonaron: 

	cd carpeta-de-proyecto
	ls
	git status

Con el comando `ls` pueden ver que este todo el repositorio alli (que de momento solo deberia estar el **README.md** sin nada sino una primera linea de texto). El comando `git status` deberia decirles algo como "branch up to date with master", o algo parecido. Ahora, pueden remplazar ese documento **README.md** vacio con el que llevan trabajando. Como cambiar solo una linea de un documento implica que le hicieron un cambio al repo, entonces veamos si **git** lo registro: 

	git status

Este mismo comando de `git status` ahora deberia indicar que el archivo **README.md** contiene cambios. Deberia salirles algo asi: 
 
 ![status](https://github.com/pesalerno/MingaGenomica2019/blob/master/fotos/gitstatus.png)


Luego, queremos agregar el archivo. Para eso, hacemos: 

	git add README.md
	git status

Se agrego? Tercero, nos "comprometemos al cambio" del documento que agregamos en el paso anterior (el lenguage git es un poco extraño, pero asi son las cosas): 

	git commit -m 'agreguen cualquier mensaje aqui'
	git status

El paso anterior es algo medio raro, pero tiene sentido dado que git fue creado para programadores que estan todos trabajando a la vez en un mismo documento y sin tener mucha idea de que hace el otro... este es un paso de control... asi como: 

>ey, tu, seguro que quieres hacer esto?? seguro?!?! y que hiciste?? 

Por eso tambien lleva consigo un mensaje, para que los colaboradores lo vean. Si no agregas mensaje... no sirve el comando! Ahora, finalmente estamos listos para mandar los cambios al repositorio. Este paso es mucho mas complicado cuando mas de una persona trabaja en un repo, pero por lo pronto es bien simple (aunque no menos raro): 

	git push origin master

La primera vez que hacen este comando, la compu por lo general les pide el password, porque git quiere saber que tu tienes permisos para tomar decisiones acerca de ese repositorio. Si no eres el dueño, entonces le pregunta al dueño (por email) si tu quiere o no aceptar los cambios. Ningun cambio es aprobado si no es por el "branch master"... de nuevo con los terminos raros de git. 

>Cuando parezca que el "push" funciono desde el terminal, vayan a la web y refresquen la pagina, a ver si se incorporaron y registraron los cambios!! 



La idea seria tambien montar una especie de "roadmap" o camino de todos los pasos desde "raw" hasta analisis finales, y por ende tambien seria bueno subir los archivos de barcodes y otros documentos y ponerles links activos en el mismo repositorio de git (como esta en la pagina de la minga genomica). Se atreven a montar mas documentos y ponerles links activos en git?? como lo harian? 


>happy gitting! 