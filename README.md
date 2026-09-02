# AlgLixeiraEletronica
Este código gerencia um sistema automatizado de abertura e fechamento de tampa (como uma lixeira inteligente) usando Arduino, um sensor ultrassônico (HC-SR04) para detectar aproximação, um servo motor para acionar o mecanismo e um LED indicador de status.

O circuito monitora continuamente a distância à sua frente. Quando uma mão ou objeto se aproxima a menos de 50 cm, o LED acende, a tampa se abre de forma suave, permanece aberta por 3 segundos, fecha suavemente e aguarda o objeto ser removido antes de liberar uma nova detecção.

Explicação Detalhada por Módulos
1. Bloco de Configurações
Define os pinos do Arduino e as variáveis ajustáveis do projeto:

Pinos: Sensor ultrassônico nos pinos 5 (Trig) e 6 (Echo), servo motor no pino 7 e LED no pino 10.

Posições do Servo: 95° (tampa fechada) e 0° (tampa aberta).

Parâmetros de Operação: Distância limite de acionamento (50 cm), tempo de permanência aberta (3000 ms) e velocidade do movimento.

2. Medição e Filtragem de Distância
medirDistancia(): Dispara um pulso ultrassônico de 10 microssegundos e mede o tempo de resposta do eco. Converte esse tempo em centímetros dividindo por 58. Se não houver resposta dentro de 30 milissegundos, retorna o código 999 (sem leitura).

calcularDistanciaMedia(): Realiza 3 medições consecutivas e calcula a média apenas dos valores válidos (entre 0 e 400 cm). Essa técnica de filtragem evita leituras falsas causadas por ruídos temporários no sensor.

3. Controle do Servo Motor e Tampa
moverServoSuavemente(): Avança ou recua o motor grau a grau com um pequeno delay (velocidadeServo). Isso evita o tranco inicial que ocorreria se o motor tentasse ir direto de 0° a 95° instantaneamente.

abrirTampa(): Executa a sequência completa:

Liga o LED.

Conecta o servo (servo.attach).

Move suavemente de 95° para 0°.

Aguarda 3 segundos.

Move suavemente de volta para 95°.

Desconecta o servo (servo.detach()).

Desliga o LED.

Destaque de projeto: Desconectar o servo com servo.detach() ao terminar o movimento é uma excelente prática. Isso impede que o motor fique zumbindo, esquentando ou consumindo bateria desnecessariamente enquanto a tampa está parada.

4. Trava de Segurança e Fluxo Principal
aguardarObjetoSair(): Após fechar a tampa, esta função mantém o sistema em pausa enquanto o objeto ou mão continuar perto do sensor (a menos de 50 cm), evitando que a tampa fique abrindo e fechando repetidamente sem parar. Há um tempo limite de segurança de 10 segundos para liberar o sistema caso algo fique fixo na frente.

setup(): Inicializa a comunicação serial a 9600 baud, define as entradas e saídas dos pinos e garante que a tampa inicie na posição fechada.

loop(): Ciclo principal que obtém a distância média, exibe a leitura no Monitor Serial e chama o ciclo de abertura caso um objeto seja detectado na faixa configurada.

Você pretende alterar este código para algum hardware específico ou gostaria de ajustar os tempos e ângulos de abertura para o seu projeto?
