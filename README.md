# MPComm
Funcionamento: Neste sistema, cada processo envia mensagens de "handshake" para indicar prontidão e, em seguida, ocorre a troca de mensagens de dados. As mensagens são registradas em arquivos de log individuais e, posteriormente, comparadas por um servidor central.

Inicialização: Cada peer inicia o processo enviando mensagens de "handshake" para indicar sua disposição em se comunicar.
Os handshakes são trocados entre os peers para garantir a disponibilidade de todos. A recepção de mensagens somente é iniciada após a conclusão dos handshakes de todos os processos (handShakeCount = Numero de processos).

Componentes em ação: Cada peer cria uma thread chamada MsgHandler, responsável por lidar com a recepção de mensagens de dados provenientes dos outros peers.
As mensagens são recebidas por meio de um socket e são armazenadas em arquivos de log individuais para cada peer.
Após a recepção de todas as mensagens de dados, os peers enviam suas mensagens para um servidor de comparação utilizando um socket dedicado.
O servidor compara as mensagens de todos os peers para verificar a consistência das sequências.
Quando os peers não têm mais mensagens para enviar, eles enviam mensagens de "stop".
A comunicação é encerrada assim que todas as mensagens de "stop" forem emitidas por todos os peers.

Conclusão: É importante notar que essa abordagem pode apresentar limitações em termos de escalabilidade e tratamento de falhas. A falta de coordenação pode levar a problemas de sincronização, como observado em um cenário de laboratório no qual diferentes peers recebem as mensagens em ordens distintas, resultando em inconsistências nos registros de mensagens e na comparação realizada pelo servidor central.
Uma solução potencial para esse desafio seria implementar um mecanismo de ordenação das mensagens, como o multicast totalmente ordenado, a inserção de carimbos de timestamp nas mensagens (que requereria sincronização temporal entre os peers) ou a adoção de relógios lógicos (uma abordagem mais orientada à coerência na ordenação do que ao próprio timestamp).
