\documentclass{article}
\usepackage[english]{babel}
\usepackage[letterpaper,top=2cm,bottom=2cm,left=3cm,right=3cm,marginparwidth=1.75cm]{geometry}

% Useful packages
\usepackage{amsmath}
\usepackage{graphicx}
\usepackage[colorlinks=true, allcolors=blue]{hyperref}
\usepackage{algpseudocode}
\usepackage{mismath}
\usepackage{algorithm}

\begin{document}


\section*{Solución programación voraz}


\subsection{Descripción de la idea.}
Para la producción en lineas de ensamblaje se pensó en ver las lineas de producción como vértices, donde los tiempos de transición y ensamblaje representarán las aristas del grafo. Se utilizó las colas de prioridad o clase Heap con el propósito de devolver la mínima arista guardada en el árbol de adyacencia, por último se opto por escoger el algoritmo de dijkstra porque debido a su gran propiedad de busqueda de los caminos mas cortos, se podia recorrer el grafo de tal forma que se guardaran las decisiones en un arreglo.


\subsection{Seudocódigo}

\begin{algorithmic}[1]
    \Procedure{dijkstra}{$g,initNode,numOp$}
       \State $distancias\gets []$
       \State $heapMinEdge\gets Heap()$
       \State $posHeap\gets 0$
       \State $padre\gets []$
       \newline
       
       \For{$i$ $in$ $list(g.keys())$}
         \State $distancias[i]\gets 10*numOp + 1$;
         \State $heapMinEdge.heap.append(Heap.newHeapNode(i, distancias[i]))$;
         \State $heapMinEdge.positions[i]\gets posHeap$;
         \State $posHeap:=heap+1$;
       \EndFor
      \newline
        
      \State $distancias[initNode]\gets 0$
      \State $heapMinEdge.changeKey(initNode, distancias[initNode])$
      \State $heapMinEdge.heapSize\gets numOp*2 + 2$
      \newline
    
      \While{$not heapMinEdge.emptyHeap()$}
       \State $u\gets heapMinEdge.getMinElement()[0]$
            \For{$j$ $in$ $g[u]$}
                 \State $ref\gets j[0]$
                 
                 \If{$heapMinEdge.in_heap(ref) and distancias[u] != 10*numOp + 1 and j[1] + distancias[u] < distancias[ref]$}
                     \State $distancias[ref]\gets j[1] + distancias[u]$
                     \State $padre[ref]\gets [u, distancias[ref]]$
                     \State $heapMinEdge.changeKey(ref, distancias[ref])$
            
                  \EndIf
            \EndFor
            
            
        \EndWhile
        \newline
        \State \textbf{return} $padre$
        \newline
    \EndProcedure
\end{algorithmic}


\subsection{Análisis de complejidad}
Para el análisis de la función Dijkstra tenemos que para primer For tomara un tiempo de O (V) [V: número de vértices] eso para poder asignarle a cada vértice una distancia en el arreglo de distancias e insertarlo dentro de binary Heap con su distancia asociada.
\newline
\newline
La cola de prioridad ( binary heap) en sus operaciones de extracción de un vértice con la menor distancia y la actualización o disminución de la distancia de un vértice en específico, nos ofrecerá un tiempo de O (log V) para cada operación por individual.
\newline
\newline
Para el caso del While este se itera sobre los vértices pertenecientes a la cola de prioridad O (V), además por cada iteración se extraerá el vértice con la menor distancia en el binary heap, proceso que toma un tiempo de O (log V), en este caso se tiene que el tiempo será O (V · Log V).
\newline
\newline
Dado que se tendrá que recorrer todas las aristas o vértices adyacentes al vértice actual se tiene que la iteración de este proceso toma un tiempo de O (E) [E: número de aristas], también cabe recordar que se realizara el proceso de cambio de distancias dentro de la cola de prioridad ( binary heap) - O (Log V) al igual que la modificación de las distancias de los vértices en el arreglo de distancias – O (1) y la asignación o modificación de los valores en el arreglo padre – O (1), para estas tres operaciones tenemos O (Log V) + O (1) + O (1) = O (Log V). En total se tiene O (E · Log V).
\newline
\newline
En la faceta final referente a la complejidad del algoritmo Dijkstra en su totalidad se puede llegar a decir que será O (V) + O (V · Log V) + O (E · Log V) = O ((V + E) · Log V).
\newline
\newline

Finalmente para el análisis de la función Solve tenemos que, como primera instancia se tiene el proceso de la creación de los grafos, a lo cual es necesario iterar n-1 veces (n: número de operaciones) para insertar nuevos vértices con sus respectivas aristas o vértices adyacentes, siendo de esta naturaleza un O (n).
\newline
El último ciclo while recorrerá la lista de adyacencia que devolvió la función Dijkstra desde el ultimo vértice hasta el vértice inicial, dando vuelta hacia atrás al nodo que referencie al actual, este proceso se realizará V/2 hasta que el vértice sea ‘init, su tiempo será O (v).
\newline
\newline
Para terminar, tomando las tres partes primordiales o esenciales de la función solve se tiene que el tiempo total será O (n) + O ((V + E) · Log V) + O (V) y sabiendo que $|V| > |n|$ entonces O ((V + E) · Log V).


\subsection{Detalles principales de la implementación}
Hardware implementado: \newline
                       Procesador Intel core i7-4510U
                       \newline
                       Memoria: 8192MB RAM
                       \newline
                       Disco solido SSD 512Gb
\newline    
\newline
Software implementado: \newline
                       Sistema operativo: Windows 10 
                       \newline
                       Visual Studio Code
\newline
\newline                      
Aspectos propios de la implementación: Se usaron las Heap o colas de prioridad como la principal estructura de datos para el algoritmo Dijkstra con el proposito de almacenar los pesos de los vértices  en un árbol para posteriormente obtener el vértice con el menor peso.
\newline
\newline
La otra estructura de datos implementada fue el diccionario para representar los grafos, porque a cada key que representaría un vértice le asignaría como valor un arreglo de tuplas de los vértices adyacentes con el peso de las aristas. Ademas también se utilizó para guardar los vértices padres del nodo del vértice actual junto con el peso acumulado mínimo.
\newline
\newline
Listas se utilizaron para guardar las elecciones de los caminos que se decidieron tomar.


\subsection{Descripción y análisis de las pruebas realizadas}
Para las pruebas realizadas pudimos observar que los tiempos de ejecución para entradas del mismo tamaño independientemente de como esté organizada la información, el tiempo no varia mucho o mantiene un rango aproximado. Por último se puede apreciar que los tiempos de ejecución incrementan cuando se incrementa los valores de entrada, aunque no es un cambio con una tendencia de cremicimiento muy alta.


\subsection{Conclusiones y aspectos para mejorar}




\end{document}
