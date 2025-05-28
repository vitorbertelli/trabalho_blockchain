
# Análise e Extensão de um Código de Mineração Blockchain

## Análise e Explicação

escrever alguma coisa

```py
class Block:
	def __init__(self,  index,  previous_hash,  timestamp,  data,  nonce=0):
		self.index = index
		self.previous_hash = previous_hash
		self.timestamp = timestamp
		self.data = data
		self.nonce = nonce
		self.hash = self.calculate_hash()
```

Esse trecho de código é a criação da classe `Block`, que é uitlizada para representar um bloco na blockchain.

-  **index:** representa a posição do bloco na blockchain.
-  **previous_hash:** atributo que armazena o hash do bloco anterior na blockchain.
-  **timestamp:** registra a hora que o bloco foi criado.
-  **data:** geralmente armazena no bloco as transações realizadas.
-  **nonce:** variável utilizada para randomizar o conteúdo do bloco, alterando o hash resultante. Iniciada como zero e incrementada sempre que tentarmos criar um hash válido.
-  **hash:** contém o hash do bloco atual.

A variável `hash` é criado atráves de um cálculo que envolve todos os dados do bloco, logo, o hash do bloco está diretamente ligado aos dados que ele armazena, fazendo com qualquer mudança em algum atributo ou no próprio hash gerasse assim um desequilibrio em toda a blockchain, o que garante a integridade e a segurança da cadeia de blocos. A função `calculate_hash` é responsável por realizar esse cálculo.

```py

def calculate_hash(self):
	value =  str(self.index)  + self.previous_hash +  str(self.timestamp)  + self.data +  str(self.nonce)
	return hashlib.sha256(value.encode()).hexdigest()

```

Essa função serve para gerar um código único, chamado hash, que representa o conteúdo de um bloco na blockchain. Para isso, ela concatena os principais dados do bloco em uma única string. Em seguida, essa string é convertida em bytes e processada com o algoritmo SHA-256, que gera um hash criptográfico. O resultado é retornado como uma string hexadecimal.

Por cada  bloco da cadeia conter o hash do bloco anterior, se alguém tentar modificar o conteúdo de um bloco, será necessário alterar também o hash do bloco seguinte, e assim por diante, até o último. Isso torna a fraude extremamente difícil, pois exige recalcular os hashes de todos os blocos seguintes.

A validação e inserção de novos blocos na blockchain é feita por meio de um processo chamado mineração. Nele, os mineradores precisam encontrar um valor chamado `nonce` que, combinado com os dados do bloco, produza um hash que atenda a um critério específico: começar com uma certa quantidade de zeros à esquerda. Esse critério é definido pela variável `difficulty`. Quanto maior a dificuldade, mais zeros são exigidos e, consequentemente, maior o esforço computacional necessário. Esse mecanismo de prova de trabalho (_proof of work_) garante a segurança da rede e impede que qualquer participante consiga inserir blocos de forma arbitrária.

```py
def mine_block(self,  difficulty,  stop_event):
	prefix =  '0'  * difficulty
	while  not self.hash.startswith(prefix):
		if stop_event.is_set():
			return
			
		self.nonce +=  1
		self.hash = self.calculate_hash()

	stop_event.set()
	print(f"Bloco minerado com nonce {self.nonce}: {self.hash}")
```

Esse método é responsável por realizar a mineração de um bloco na blockchain, utilizando o conceito de prova de trabalho. Esse mecanismo dificulta a criação de blocos falsos e garante a integridade e a segurança da blockchain, pois encontrar esse hash demanda tempo e poder computacional, enquanto a verificação é rápida e simples.

### Questionário

Breves respostas para questões propostas.

#### O que impede dois blocos de serem minerados simultaneamente?



#### Como o hash e o nonce garantem a dificuldade da mineração?