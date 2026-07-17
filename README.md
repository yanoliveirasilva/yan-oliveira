Projeto de Resolução de Cubo Mágico com Visão Computacional e Arduino
Este projeto implementa um sistema para escanear e resolver um cubo mágico (Rubik's Cube) utilizando visão computacional em Python e controle de hardware via Arduino. O sistema é capaz de identificar as cores de cada face do cubo, determinar a sequência de movimentos para resolvê-lo e, em seguida, executar esses movimentos através de um mecanismo robótico controlado pelo Arduino.

Funcionalidades
Módulo Python (Scanner e Resolvedor)
Comunicação Serial: Gerencia a comunicação bidirecional com o Arduino para enviar comandos de movimento e receber status.
Captura de Imagem: Utiliza a DroidCam (ou outra fonte de vídeo compatível com OpenCV) para capturar imagens das faces do cubo.
Processamento de Imagem:
Rotação e recorte de imagens para isolar a face do cubo.
Aplicação de algoritmo K-means para identificar as cores dominantes em cada adesivo do cubo.
Conversão para o espaço de cores HSL para maior robustez contra variações de iluminação.
Mapeamento das cores detectadas para uma notação padrão do cubo mágico.
Varredura do Cubo: Orquestra uma sequência de movimentos do Arduino para expor todas as seis faces do cubo à câmera, capturando e analisando cada uma.
Resolução do Cubo: Integra a biblioteca kociemba para calcular a sequência de movimentos mais curta para resolver o cubo a partir do estado escaneado.
Execução de Movimentos: Traduz a solução do algoritmo Kociemba em comandos específicos para o Arduino, que executa os movimentos físicos no cubo.

Módulo Arduino (Controlador de Hardware)
Controle de Servos: Gerencia três servomotores para controlar os braços que seguram e manipulam o cubo, e a base que o gira.
Movimentos Básicos: Implementa funções para:
moveArmRelease(): Posição para soltar o cubo.
moveArmHold(): Posição para segurar o cubo.
flipCube(): Gira o cubo em torno de um eixo para expor diferentes faces.
rotateBase(): Gira a base do mecanismo em direções específicas (horário/anti-horário).
Protocolo de Comunicação: Responde a comandos enviados via serial pelo Python (MOVE U, MOVE F, RESET, FINISH, PC_READY) e envia mensagens de status (READY, DONE, ERROR).

Tecnologias Utilizadas
Python 3.x: Linguagem principal para a lógica de visão computacional e resolução.
pyserial: Comunicação serial com Arduino.
Pillow (PIL): Manipulação de imagens.
NumPy: Operações numéricas e manipulação de arrays.
SciPy: Algoritmo K-means para agrupamento de cores.
OpenCV (cv2): Captura de vídeo e processamento de imagem.
kociemba: Biblioteca para resolver o cubo mágico.
Arduino (C++): Firmware para controle dos servomotores.
Biblioteca Servo.h.
DroidCam: Aplicativo para usar um smartphone como câmera IP (configurável).

Estrutura do Projeto
O projeto é composto por dois arquivos principais:

O código Python para o scanner e resolvedor do cubo, e o código C++ (sketch Arduino) para o controlador de hardware.
Código Python: Contém as classes ArduinoCommunicator e CubeScanner, além da lógica principal para a varredura e resolução.
Código C++: Contém o sketch Arduino para controlar os servomotores e responder aos comandos seriais.

Configuração
1. Configuração do Ambiente Python
Certifique-se de ter o Python instalado. Instale as bibliotecas necessárias:

pip install pyserial Pillow numpy scipy opencv-python kociemba

2. Configuração do Arduino
Abra o código C++ fornecido em sua IDE Arduino.
Conecte os servomotores aos pinos digitais do Arduino conforme definido no código (ARM1_PIN=10, ARM2_PIN=11, BASE_PIN=9).
Faça o upload do sketch para o seu Arduino.
Anote a porta serial à qual o Arduino está conectado (ex: /dev/ttyUSB0 no Linux, COM3 no Windows).

3. Configuração da Câmera (DroidCam)
Instale o aplicativo DroidCam em seu smartphone e o cliente DroidCam em seu computador.
Configure a DroidCam para transmitir vídeo via IP e anote o endereço IP e a porta (ex: http://YOUR_DROIDCAM_IP_ADDRESS:PORT/video).
Importante: Substitua "http://YOUR_DROIDCAM_IP_ADDRESS:PORT/video" no código Python pela URL real da sua DroidCam.

Uso
Ajuste de Parâmetros:
No código Python, ajuste a variável arduino_port na classe CubeScanner e na função main() do resolvedor para a porta serial correta do seu Arduino.
Ajuste a variável droidcam_url na classe CubeScanner para a URL da sua DroidCam.
Execução:
O código camera 2.py junto ao app droidcam escaneiam o cubo e o controlador de câmera manipula o cubo para escanear o cubo 
O bloco if __name__ == "__main__": no código Python demonstra um exemplo de uso para varredura do cubo.

Para iniciar a varredura e resolução (usando a lógica do resolvedor):

# Certifique-se de que o Arduino está conectado e o sketch carregado
# Certifique-se de que a DroidCam está ativa e a URL correta está configurada
 
# Exemplo de execução da lógica principal do resolvedor
# Esta parte do código espera comandos do Arduino para iniciar a varredura e resolução.
# O Arduino deve enviar "READY", "SCAN", "SOLVE" para interagir com o script Python.
# O script Python irá então se comunicar com o Arduino para escanear o cubo,
# calcular a solução e enviar os movimentos de volta.
 
# Para testar a varredura de faces individualmente (como no exemplo do CubeScanner):
# scanner = CubeScanner(arduino_port="/dev/ttyUSB0", droidcam_url="http://YOUR_DROIDCAM_IP_ADDRESS:PORT/video")
# cube_colors = scanner.scan_cube_faces()
# if cube_colors:
#     print("\n--- Resultado Final da Varredura do Cubo ---")
#     for face, colors in cube_colors.items():
#         print(f"Face {face}: {colors}")
# else:
#     print("A varredura do cubo falhou ou não produziu resultados.")
 
# Para a lógica completa de resolução (main do resolvedor):
# Você precisará de um loop de comunicação onde o Arduino inicia o processo.
# O código fornecido no notebook já inclui a função main() que lida com isso.
# Execute essa função após configurar tudo.

#     main()

Observações
A calibração dos ângulos dos servomotores e os tempos de delay() no código Arduino podem precisar de ajustes finos dependendo da sua montagem mecânica.
A detecção de cores pode ser sensível à iluminação ambiente. Garanta uma iluminação consistente para melhores resultados.
A URL da DroidCam (http://YOUR_DROIDCAM_IP_ADDRESS:PORT/video) deve ser substituída pelo endereço IP e porta corretos do seu dispositivo.
A porta serial (/dev/ttyUSB0 ou COMx) deve ser verificada e ajustada conforme o sistema operacional e a conexão do Arduino. 

