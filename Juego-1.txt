#!/bin/bash

clear
echo "Bienvenido/a, estas por iniciar una partida de Blackjack."
echo "Por favor ingresa tu nombre:"
read nombre
clear
echo "Instrucciones:"
echo "El objetivo del juego es hacer 21 puntos."
echo "Si el jugador excede el puntaje requerido se considerará perdida."
echo "Se jugará contra la maquina por lo que el ganador se decidirá quien más se acerque al puntaje."
echo "O quien consiga 21 puntos."
echo "Las cartas tendran el siguiente valor sin importar el palo:"
echo "A tendrá dos valores según decida el jugador. Los valores que puede tomar son 1 y 11."
echo "Las cartas del 1 al 10 tendrán su mismo valor."
echo "J, Q y K tendrán un valor de 10"
echo "Espere mientas inicia el juego..."
sleep 5
clear

validadorGanador=1
validadorJuego=1
contador=1
val=0
num1=1
num2=1
total=0
while [ $validadorJuego -eq 1 ]; do
        echo "Inicia el juego..."
        while [ $validadorGanador -eq 1 ]; do
                echo "Juego $contador"
                echo "La máquina reparte 1 carta..."
                sleep 2
                carta1=$(echo $(($RANDOM%13)))
                maq=$(echo $(($RANDOM%13)))
                if [ $carta1 -eq 0 ]; then
                        while [ $val -eq 0 ]; do
                                echo "La carta es A."
                                echo "Qué valor quiere asignar a la carta? (1 u 11)"
                                read valor
                                if [ $valor -eq 1 ]; then
                                        carta1=$valor
                                        val=1
                                elif [ $valor -eq 11 ];then
                                        carta1=$valor
                                        val=1
                                else
                                        echo "Valor incorrecto"
                                fi
                        done
                        val=0
                elif [ $carta1 -eq 11 ]; then
                        echo "La carta es J."
                        carta1=10
                elif [ $carta1 -eq 12 ]; then
                        echo "La carta es Q."
                        carta1=10
                elif [ $carta1 -eq 13 ]; then
                        echo "La carta es K."
                        carta1=10
                else
                        echo "La carta es $carta1"
                fi

                if [ $maq -eq 0 ]; then
                        maq=11
                elif [ $maq -eq 11 ]; then
                        maq=10
                elif [ $maq -eq 12 ]; then
                        maq=10
                elif [ $maq -eq 13 ]; then
                        maq=10
                fi
                sleep 2
                let totalj=$totalj+$carta1
                let totalm=$totalm+$maq
                echo "Puntaje:  $totalj"

                echo "Desea pedir otra carta? (s/n)"
                read opcion
                if [ $opcion == "n" -o $opcion == "N"  -o $opcion == "No" -o $opcion == "no" ]; then
                        validadorGanador=0
                        clear
                        echo "Resultados:"
                        if [ $totalj -eq 21 -a $totalm -ne 21 ]; then
                                echo "El jugador $nombre, ha ganado!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="ganado"
                                sleep 3
                        elif [ $totalm -eq 21 -a $totalj -ne 21 ]; then
                                echo "El jugador $nombre, ha perdido!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="perdido"
                                sleep 3
                        elif [ $totalj -lt 21 -a $totalj -gt $totalm ]; then
                                echo "El jugador $nombre, ha ganado!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="ganado"
                                sleep 3
                        elif [ $totalj -gt 21 -a $totalj -lt $totalm ]; then
                                echo "El jugador $nombre, ha ganado!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="ganado"
                                sleep 3
                        elif [ $totalj -lt 21 -a $totalm -gt 21 ]; then
                                echo "El jugador $nombre, ha ganado!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="ganado"
                                sleep 3
                        elif [ $totalj -eq $totalm ]; then
                                echo "Es un empate!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="empatado"
                                sleep 3
                        else
                                echo "El jugador $nombre, ha perdido!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="perdido"
                                sleep 3
                        fi
                        if [ -d Resultados ]; then
                                cd Resultados
                                echo -e "Resultado:" "\n" >> Resultados_Juegos.txt
                                echo "El jugador $nombre, ha $res!" >> Resultados_Juegos.txt
                                echo "Puntaje del jugador: $totalj" >> Resultados_Juegos.txt
                                echo "Puntaje de la máquina: $totalm" >> Resultados_Juegos.txt
                                echo -e "Fecha:" $(date '+%Y/%m/%d') "\n" >> Resultados_Juegos.txt
                                base64 Resultados_Juegos.txt > Resultados_Juegos_encriptado.txt
                                cd ..
                        else
                                mkdir Resultados
                                cd Resultados
                                echo -e "Resultado:" "\n" >> Resultados_Juegos.txt
                                echo "El jugador $nombre, ha $res!" >> Resultados_Juegos.txt
                                echo "Puntaje del jugador: $totalj" >> Resultados_Juegos.txt
                                echo "Puntaje de la máquina: $totalm" >> Resultados_Juegos.txt
                                echo -e "Fecha:" $(date '+%Y/%m/%d') "\n" >> Resultados_Juegos.txt
                                base64 Resultados_Juegos.txt > Resultados_Juegos_encriptado.txt
                                cd ..
                        fi
                elif [ $opcion == "s" -o $opcion == "S" -o $opcion == "si" -o $opcion == "Si" ]; then
                        if [ $totalj -gt 21 ]; then
                                clear
                                echo "El jugador ya no puede pedir más cartas"
                                echo "Resultado:"
                                echo "El jugador $nombre, ha perdido!"
                                echo "Puntaje del jugador: $totalj"
                                echo "Puntaje de la máquina: $totalm"
                                res="perdido"
                                validadorGanador=0
                                if [ -d Resultados ]; then
                                        cd Resultados
                                        echo -e "Resultado:" "\n" >> Resultados_Juegos.txt
                                        echo "El jugador $nombre, ha $res!" >> Resultados_Juegos.txt
                                        echo "Puntaje del jugador: $totalj" >> Resultados_Juegos.txt
                                        echo "Puntaje de la máquina: $totalm" >> Resultados_Juegos.txt
                                        echo -e "Fecha:" $(date '+%Y/%m/%d') "\n" >> Resultados_Juegos.txt
                                        base64 Resultados_Juegos.txt > Resultados_Juegos_encriptado.txt
                                        cd ..
                                else
                                        mkdir Resultados
                                        cd Resultados
                                        echo -e "Resultado:" "\n" >> Resultados_Juegos.txt
                                        echo "El jugador $nombre, ha $res!" >> Resultados_Juegos.txt
                                        echo "Puntaje del jugador: $totalj" >> Resultados_Juegos.txt
                                        echo "Puntaje de la máquina: $totalm" >> Resultados_Juegos.txt
                                        echo -e "Fecha:" $(date '+%Y/%m/%d') "\n" >> Resultados_Juegos.txt
                                        base64 Resultados_Juegos.txt > Resultados_Juegos_encriptado.txt
                                        cd ..
                                fi
                                sleep 3
                        else
                                echo "Se le repartirá otra carta, espera..."
                                sleep 3
                        fi
                else
                        echo "Opcion incorrecta, como castigo se le dará otra carta"
                        sleep 3
                fi
                ((contador++))
        done
        clear
        echo "Desea volver a jugar ? (s/n)"
        read opcion
        if [ $opcion == "n" -o $opcion == "N"  -o $opcion == "No" -o $opcion == "no" ]; then
                validadorJuego=0
                clear
                echo "Saliendo..."
                sleep 2
                echo "Muchas gracias por jugar, vuelva pronto jugador $nombre"
                sleep 3
                clear
        elif [ $opcion == "s" -o $opcion == "S" -o $opcion == "si" -o $opcion == "Si" ]; then
                validadorJuego=1
                clear
                echo "Reiniciando el juego..."
                contador=1
                validadorGanador=1
                totalm=0
                totalj=0
                sleep 3
                clear
        else
                clear
                echo "Opcion no válida"
                echo "Saliendo del juego..."
                validadorJuego=0
                sleep 3
                clear
        fi
done