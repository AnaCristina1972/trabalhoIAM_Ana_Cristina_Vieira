# Trabalho de Introdução de aprendizado de máquina
Enunciado:

Utilizando o vídeo da montanha russa e com as técnicas de detecção de cores mostradas em laboratório, gere códigos em python com as seguintes finalidades:

1) Identifique no vídeo, de forma dinâmica (usando uma estrutura de repetição para ler cada frame do vídeo), em qual frame começa a aparecer a cor vermelha. Mostre a imagem do frame pelo código, assim como o número do frame (contador) em que isso ocorre.

2) Gere uma cópia de todos os frames, isolando somente a parte vermelha, do início ao fim do vídeo.



   Para este trabalho foi criado dois códigos iguais, porém distintos na sua saída. abaixo estar os códigos desenvolvidos, onde as referencias estar em aulas de cursos passados, ministrados pelo professor Felipe da UFAM/ICET.
    Resolução:

   Nesse momento foi importado as bibliotecas que iremos precisar, foi carregado o vídeo trabalhado por nome mont_russa.mp4 e por fim, criando e inicializado um contador, para representar a numeração do frame.

                                                                              import matplotlib.pyplot as plt
                                                                              from pylab import *
                                                                              from PIL import ImageOps
                                                                              import cv2
                                                                              import numpy as np
                                                                              from PIL import Image
                                                                              from io import BytesIO
                                                                              import IPython.display as display
                                                                              
                                                                              video_path = '/content/mont_russa.mp4'# fazendo  o upload do video, estando ele e o OpenCV na nuvem
                                                                              video = cv2.VideoCapture(video_path)
                                                                              
                                                                              #video_path = 'mont_russa.mp4'     # fazendo  o upload do video, estando ele e o OpenCV na máquina
                                                                              #video = cv2.Videovideoture(video_path)
                                                                              
                                                                              contador_frame = 0       # Inicializando as variavéis que vai ser usadas nos dois codigos
   CÓDIGO COM RGB/BGR
No primeiro código, a detecção da cor vermelha é realizada diretamente no espaço de cores RGB (BGR no OpenCV) dos frames do vídeo. Isso significa que a detecção é baseada nas intensidades dos canais Vermelho, Verde e Azul de cada pixel. No entanto, esse método pode ser sensível a variações na iluminação, sombras e outras condições que afetam os canais de cores RGB. Portanto, a precisão da detecção da cor vermelha pode ser afetada por esses fatores, tornando-o mais propenso a falsos positivos, como podemos perceber nos prints das imagens geradas.
                                                                               while True:
                                                                                ret, frame = video.read()
                                                                                if ret:
                                                                                    lower_red = np.array([0, 0, 100])       # Intervalos de cores vermelhas em BGR
                                                                                    upper_red = np.array([100, 100, 255])
                                                                                    red_mask = cv2.inRange(frame, lower_red, upper_red)    # Máscara para a cor vermelha
                                                                                    print(f"Cor vermelha detectada no frame {contador_frame}")
                                                                            
                                                                                    red_part = cv2.bitwise_and(frame, frame, mask=red_mask) # Isola a parte vermelha do frame
                                                                            
                                                                                    image = Image.fromarray(cv2.cvtColor(red_part, cv2.COLOR_BGR2RGB))   # Salvando a imagem para download
                                                                                    image_bytes = BytesIO()
                                                                                    image.save(image_bytes, format='PNG')
                                                                                    image_bytes.seek(0)
                                                                                    display.display(display.Image(data=image_bytes.read()))  # Printa a imagem
                                                                            
                                                                                    contador_frame += 1
                                                                                else:
                                                                                    break  # Saia do loop
                                                                            video.release()
   CÓDIGO CONVERTIDO PARA HSV
No segundo código, os frames do vídeo são convertidos para o espaço de cores HSV (Matiz-Saturação-Valor) antes da detecção da cor vermelha. No espaço de cores HSV, a detecção de cores é mais intuitiva, pois o matiz (Hue) representa diretamente a cor, independentemente do brilho. Isso torna o método menos sensível às variações na iluminação, tornando-o mais robusto em diferentes condições de iluminação. Além disso, o espaço de cores HSV é menos propenso a falsos positivos, tornando a detecção da cor vermelha mais precisa e confiável em uma variedade de cenários, como mostra os prints gerados.

                                                                            while True:
                                                                               
                                                                                ret, frame = video.read()    # Lendo de um frame
                                                                                if ret:
                                                                                  hsv_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)  # Convertendo o frame para o espaço de cores HSV
                                                                                  lower_red = np.array([0, 100, 100])                 # Definindo os intervalos de cores vermelhas em HSV
                                                                                  upper_red = np.array([10, 255, 255])
                                                                                  red_mask = cv2.inRange(hsv_frame, lower_red, upper_red)  # Isso é uma máscara para a cor vermelha
                                                                                  print(f"Cor vermelha detectada no frame {contador_frame}")
                                                                                  red_part = cv2.bitwise_and(frame, frame, mask=red_mask)     # Isolando a parte vermelha do frame
                                                                                  image = Image.fromarray(cv2.cvtColor(red_part, cv2.COLOR_BGR2RGB))  #Salvando e convertendo a imagem
                                                                                  image_bytes = BytesIO()
                                                                                  image.save(image_bytes, format='PNG')
                                                                                  image_bytes.seek(0)  
                                                                                  display.display(display.Image(data=image_bytes.read()))    # Exiba a imagem
                                                                                  contador_frame += 1
                                                                                else:
                                                                                    break  # Saindo do loop se não houver mais frame
                                                                            video.release()

   ![imagemHSV](https://github.com/AnaCristina1972/trabalhoIAM_Ana_Cristina_Vieira/assets/88814404/11795c2b-4e26-4e87-9a00-cea862fc5571)
   As sa´sdas resultantes desses dois códigos são totalmente diferentes, portanto, a escolha entre esses métodos depende das necessidades específicas da aplicação.
   Abaixo, estar  as imagens:


   
