# automacao-planta-python01
Projeto simples que simula irrigação automática e evolução de uma planta, aplicando conceitos de automação. ⚙️💧🌳

No vídeo mostra apenas alguns casos, mas o programa reconhece quando a planta esta morrendo, caso o controlador seja desligado a saude da planta começa cair e etc. 
![Demonstração](518392b551ff4a54957c7fa6143a2b91.gif)

```python
''' Simulador de Planta Automatizada – Projeto em Python
Autor: José Gabriel de Jesus

Descrição: Criei um simulador simples que mostra o crescimento de uma planta controlada automaticamente. 
O sistema “detecta” chuva, decide quando ligar a irrigação e faz a planta evoluir de semente até árvore.'''

import random
import time
'''Variaveis criadas para mostrar: Evolução da plantação, dia, saude da plantação, regação
e o quanto a plantação evoluiu'''
evoluir = 0
dia = 0
turno = 0
saude = 50
regar = 0
up_do_dia = 0

while True:
    sensor_chuva = 0 #sensor que vai enviar sinal logico para o controlador se estiver chovendo.
    probabilidade_de_chuva = random.randint(0,7) #random para chover aleatoriamente.
    
    
    '''logica para chuva e sensor'''
    if probabilidade_de_chuva == 3 or probabilidade_de_chuva == 6: # Logica se chover
        controlador = "o"
        print ("🌧️🌧️🌧️🌧️🌧️🌧️🌧️🌧️🌧️🌧️🌧️")
        print("=========================================================================================")
        print ("Sensor enviando nivel logico alto para o controlador...")
        time.sleep(1)
        print("Sinal reconhecido pelo controlador...")
        time.sleep(1.5)
        print ("Controlador desligado por estar chovendo")
        sensor_chuva = 1
        up_do_dia = random.randint(5,15)
    else: #Logica se não chover
        print("=========================================================================================")
        while True: #Loop infinito para concertar se der caso o usuario digite uma letra invalida.
            controlador = input ('Digite "O" para deixar controlador ligado e "X" para desligar \n=========================================================================================')
            if controlador == 'x' or controlador == 'X':
                up_do_dia = 0
                break
            elif  controlador == 'o' or controlador == 'O':
                up_do_dia = random.randint(5,15)
                print("=========================================================================================")
                print("Controlador ligado, dia de sol")
                break
            else: #Eu poderia usar o try e o except, mas eu achei essa forma melhor.
                print('O DIGITO TEM QUE SER A LETRA "X" OU "O"')
                print("=========================================================================================")
        
        
        
    evoluir += up_do_dia #A evolução recebe a porcentagem do quanto a planta cresceu
    dia += 1 #Cada vez que rolar uma simulação, aumenta um dia
    
    
    
    if sensor_chuva == 1: 
        '''parte visual se chover ou não'''
        print (f'Dia {dia} 🌦️')
    else:
        '''parte visual se chover ou não'''
        print (f'Dia {dia} ☀️')      
    print("=========================================================================================")
    time.sleep(0.5)
    print (f'A evolução está em {evoluir}%') #Mostrar o quanto a plantação evoluiu.
    print("=========================================================================================")
    time.sleep(0.5)
    saude += 10 # cada simulação recebe 10% de saude
    
    
    '''Organização do codigo'''
    if saude == 110:
        saude -= 10
    time.sleep(0.5)
    print (f"saude está em {saude}%")
    
    
    '''Avisos sobre a planta'''
    if 10 <= saude < 30: #Caso a planta esteja entre 10% e 30% de saude, o controlador avisa.
        print("=========================================================================================")
        time.sleep(0.5)
        print ("ATENÇÃO! LIGAR O CONTROLADOR. SAÚDE EM ESTADO CRÍTICO. ⚠️⚠️⚠️")
        print("=========================================================================================")
    elif saude < 10: #Saude menor que 10% a plantação morre e a simulação acaba.
        print("=========================================================================================")
        time.sleep(0.5)
        print ("PLANTAÇÃO MORREU. 🪾🍂")
        print("=========================================================================================")
        break
    elif saude >= 30: #Se a saude é maior ou igual a 30 a saude é estavel.
        print("=========================================================================================")
        time.sleep(0.5)
        print ("PLANTA COM SAUDE")
        print("=========================================================================================")
    time.sleep(0.5)
    
    
    '''classe da planta'''
    class Planta: #Aqui fala as definições em que estagio de crescimento está a planta.
        def __init__ (self):
            '''Definição de evolução'''
            self.semente = evoluir <= 25
            self.broto = 25 < evoluir <= 50
            self.planta = 50 < evoluir <= 75
            self.arvore = evoluir > 75
            self.dia = turno = 1 
            self.noite = turno < 1
        def controlador (self):
            '''logica para retornar o estagio da plantaçãp'''
            if evoluir <= 25: #Se a evolução da planta for menor ou igual 25 a plantação é uma sementa
                self.semente = True
                return "Semente 🥔"
            elif 25 < evoluir <= 50: #Se a evolução da planta estiver entre 25 (não inteiro) e igual ou menor que 50 é um broto.
                self.broto = True
                return "Broto 🌱" 
            elif 50 < evoluir <= 75: #Se a evolução da planta estiver entre 50 (não inteiro) e igual ou menor que 75 é uma planta.
                self.planta = True
                return "Planta 🪴"
            elif evoluir > 75: #Se a evolução for maior que 75, é uma arvore (estagip final).
                self.arvore = True 
                return "Arvore 🌳"
                
                
    '''Logica'''
    a = Planta()
    '''Caso o usuario ligue ou desligue o controlador'''
    if controlador == 'x' or controlador == 'X': #Caso o usario desligue a saude da planta cai em 20 que subtrai da saude inicial + o up_do_dia.
        time.sleep(0.5)
        saude -= 20
        evoluir -= up_do_dia
        evolucao = 0
    elif  controlador == 'o' or controlador == 'O': #Caso o usuario deixar ligado o controlodor rega a planta e segue funcionandp
        time.sleep(0.5)
        print ("REGADA COM SUCESSO")
        print("=========================================================================================")
        evolucao = up_do_dia
    time.sleep(0.5)
    
    '''Mostrar os dados do dia'''
    print (f"ESTA DE NOITE. 🌙 \n=========================================================================================\n| Relatório geral do dia: {dia} \t|Estagio: {a.controlador()} | Saúde final: {saude}% | Evolução {evolucao}% |")
    print("=========================================================================================")
    print (" ")
    print (" ")
    print (" ")
    time.sleep(1)
    if saude == 90:
        saude += 10
