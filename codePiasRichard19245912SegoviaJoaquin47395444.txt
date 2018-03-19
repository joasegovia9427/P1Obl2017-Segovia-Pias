////UDE - TRABAJO OBLIGATORIO PROG1 LIC.INFO.Nocturno2017 - Docente: Juan Garcia
////Integrantes: Pías, Richard - 1.924.591-2 ; Segovia, Joaquín - 4.739.544-4
#include <stdio.h>
#include <stdlib.h>
const int M=3; //jugadores
const int K=10; //monedas iniciales para apostar
const int N=3; //rondas de juego
const int C=5; //cantidad de numeros entre 0 y C-1
const int LARGO_NOMB=80; //largo maximo de los nombres
typedef char String[LARGO_NOMB]; //tipo de array nombres para strings
typedef String Jugadores[M]; //array que guarda los strings de nombres
typedef int Monedas[M]; //array que guarda los puntajes
typedef enum{FALSE,TRUE}boolean;
typedef int Tablero[M][C]; //tipo mat de Filas=Mjugadores x Columnas=#numParaApostar
int main(){
    int rondaActual=0,jugadorActual=0,monedasApostadas=0,numeroAapostar=0,i=0,j=0,numSorteado=0,jugadorGanador=0;
    char letra,palabra,dibujo;
    String arreNombre;
    Jugadores arreJugadores;
    Monedas arreMonedas;
    Tablero matTablero;
    boolean sinMonedas=FALSE, noPuedeApostar=FALSE;
    printf("********<<< JUEGO DE APUESTAS >>>********\n");
    printf("********<<<    Bienvenidos    >>>********\n\n");
////PARTE 1 - Registro de jugadores
    printf("Registro inicial de %d participantes\nIngresara los nombres separandolos con enter\n",M);

    for (i=0;i<M;i++)
    {//for de carga
        printf("\nIngrese el nombre del jugador nro %d y para finalizar termine con enter:",i+1);
        scanf("%c",&letra);
        j=0;
        while (j<LARGO_NOMB-1 && letra != '\n')
        {
            arreNombre[j] = letra;
            arreJugadores[i][j]=letra;
            j++;
            scanf("%c",&letra);
        }
        arreNombre[j]='\0';
        arreJugadores[i][j]='\0';
        fflush(stdin);
        printf("El nombre ingresado fue:");
        j=0;
        while (arreNombre[j] != '\0')
        {
            printf("%c",arreNombre[j]);
            j++;
        }
        printf(".\n");
        fflush(stdin);
    }
    for (i=0;i<M;i++)
    {//for de carga inicial de monedas
        arreMonedas[i]=K;
        fflush(stdin);
    }
    fflush(stdin);
    printf("\n\nDATOS DE LOS %d PARTICIPANTES:",M);
    for (i=0;i<M;i++)
    {//for de muestra
        printf("\nEl nombre del participante nro%d es: ",i+1);
        j=0;
        while (arreJugadores[i][j] != '\0')
        {
            printf("%c",arreJugadores[i][j]);
            j++;
        }
        printf(".\nCon la cantidad de %d monedas.\n",arreMonedas[i]);
        fflush(stdin);
    }
    ////PARTE 2 - Realizacion de las rondas
    do
    {//  por rondas
        printf("\n******************\n");
        printf("\n\nRONDA NUMERO %d \n",rondaActual+1);
        //limpieza de trablero
        for (i=0;i<M;i++)
            for (j=0;j<C;j++)
                matTablero[i][j]=0;
        //carga de las M monedas a los x numeros
        for (jugadorActual=0;jugadorActual<M;jugadorActual++)
        {
            do
            {
                printf("\nJugador %d, ingrese la cantidad de monedas que quieres aportar:", jugadorActual+1);
                scanf("%d",&monedasApostadas);
                noPuedeApostar=FALSE;
                if (monedasApostadas > arreMonedas[jugadorActual] || monedasApostadas < 0)
                {
                    noPuedeApostar=TRUE;
                    printf("\nError: El valor ingresado debe estar entre 0 y sus monedas disponibles(%d)\n",arreMonedas[jugadorActual]);
                }

            } while (noPuedeApostar == TRUE);
            do
            {
                printf("\nIngrese el numero al que le quiere apostar apostar entre 0 y %d, incluyendolos:",C-1);
                scanf("%d",&numeroAapostar);
                noPuedeApostar=FALSE;
                if (numeroAapostar<0 || numeroAapostar>C-1)
                {
                    noPuedeApostar=TRUE;
                    printf("\nError: El numero a apostar no es valido, debe estar entre 0 y %d\n",C-1);
                }

            } while (noPuedeApostar == TRUE);
            // DESCUENTO DE LO APOSTADO
            arreMonedas[jugadorActual] = arreMonedas[jugadorActual] - monedasApostadas;
            printf("\nUsted apostara %d monedas al numero %d\n\n",monedasApostadas,numeroAapostar);
            matTablero[jugadorActual][numeroAapostar]=monedasApostadas;
        }
        printf("\n\nDATOS DE LOS %d PARTICIPANTES TRAS APOSTAR:",M);
        for (i=0;i<M;i++)
        { //for de muestra
            printf("\nEl nombre del participante nro%d es: ",i+1);
            j=0;
            while (arreJugadores[i][j] != '\0')
            {
                printf("%c",arreJugadores[i][j]);
                j++;
            }
            printf(".\nCon la cantidad de: %d monedas.\n",arreMonedas[i]);
            fflush(stdin);
        }
        //comienza juego
        numSorteado = 0;
        numSorteado =  rand() % C;
        printf("\n\nNumero SORTEADO para la RONDA %d es :  %d",rondaActual+1,numSorteado);
        for (jugadorActual=0; jugadorActual<M; jugadorActual++)
        {
             for (j=0;j<C;j++)
             {
                if (j == numSorteado)
                { // Ganador
                    if (matTablero[jugadorActual][j] != 0)
                    {
                        arreMonedas[jugadorActual] = arreMonedas[jugadorActual] + (matTablero[jugadorActual][j] * 2);
                        printf("\n\nEl Jugador Numero %d gano en esta RONDA\n",jugadorActual+1);
                    }
                 }
             }
        }
        printf("\n\n\nDATOS DE LOS %d PARTICIPANTES TRAS APOSTAR:",M);
        for (i=0;i<M;i++)
        {//for de muestra
            printf("\nEl nombre del participante nro%d es: ",i+1);
            j=0;
            while (arreJugadores[i][j] != '\0')
            {
                printf("%c",arreJugadores[i][j]);
                j++;
            }
            printf(".\nCon la cantidad de %d monedas.\n",arreMonedas[i]);
            fflush(stdin);
        }
        jugadorActual=0;
        do
        {// verificacion de monedas
            if (arreMonedas[jugadorActual]==0)
                sinMonedas = TRUE;
            jugadorActual++;
        } while (jugadorActual<M && sinMonedas != TRUE);
        rondaActual++;
    } while (rondaActual<N && sinMonedas != TRUE);
////PARTE 3 - Finalizacion de juego
    printf("\n\n**********************************************");
    jugadorGanador = 0;
    for (jugadorActual=1; jugadorActual<M; jugadorActual++)
        if (arreMonedas[jugadorActual]>arreMonedas[jugadorActual-1])
            jugadorGanador = jugadorActual;
    if (arreMonedas[jugadorGanador] != 0)
    {
        printf("\n\n*******<<< EL GANADOR DEL JUEGO FUE >>>*******\n");
        printf("Nombre: ");

        j=0;
        while (arreJugadores[jugadorGanador][j] != '\0')
        {
            printf("%c",arreJugadores[jugadorGanador][j]);
            j++;
        }
        printf(".\nCon la cantidad de %d monedas.\n",arreMonedas[jugadorGanador]);
    }
    else
    {
        printf("\n\n*******<<<  NO HUBIERON GANADORES   >>>*******\nTodos terminaron con 0 monedas\n");
    }
    printf("\n*******<<<      FIN DEL JUEGO       >>>*******\n");
    printf("\n*******<<<**************************>>>*******\n\n");
    for (j=176;j<179;j++)
    {
        printf("\n");
        for (i=0;i<85 ;i++ )
        {
            printf("%c",j);
        }
    }
    printf("\n%c",201);
    for (i=0;i<84 ;i++ ){
        printf("%c",205);
    }
    printf("\n%c<<<*********************<<<      About this game:    >>>************************>>>%c",186,186);
    printf("\n%c<<<UDE - TRABAJO OBLIGATORIO PROG1 LIC.INFO.Nocturno 2017 - Docente: Juan Garcia>>>%c",186,186);
    printf("\n%c<<<#proudlyDevelopedBy: Pias, Richard-1.924.591-2 & Segovia, Joaquin-4.739.544-4>>>%c",186,186);
    printf("\n%c<<<*****************************************************************************>>>%c",186,186);
    printf("\n%c",200);
    for (i=0;i<84 ;i++ ){
        printf("%c",205);
    }
    for (j=176;j<179;j++)
    {
        printf("\n");
        for (i=0;i<85 ;i++ )
        {
            printf("%c",j);
        }
    }
}
////UDE - TRABAJO OBLIGATORIO PROG1 LIC.INFO.Nocturno2017 - Docente: Juan Garcia
////Integrantes: Pías, Richard - 1.924.591-2 ; Segovia, Joaquín - 4.739.544-4
