# PROGRAMAÇÃO ORIENTADA A OBJETOS COM MICROPYTHON - Aula 01

**Prof. Dr. Humberto A. P. Zanetti**

**Conteúdo:**
+ O que é POO?
  - O que são classes e objetos?
+ Atributos
+ Métodos
+ Implementando em MicroPython

---

## O que é POO (Programação Orientada a Objetos)?

A Programação Orientada a Objetos (POO) é um **paradigma** de programação que se basea no projeto do código através de uma organização de **classes** e a utilização dessas classes por meio de **objetos**.  
O  termo **objeto** vem de uma tentativa de modelar um software fazendo uma analogia com o **mundo real**, o mundo físico. Isso se apresenta como uma meia-verdade, pois em uma modelagem POO representamos também **conceitos abstratos**, como **conta bancária** ou **relatório**.
Esse paradigma é encontrado na maioria das linguagens e ainda hoje ainda é a maneira mais comum em projetos de software, inclusive sendo essencial para uso de bibliotecas e padrões de projetos.

**O que são classes e objetos?**

+ **Classe:** Define um modelo (molde) para criar objetos.
+ **Objeto:** Uma instância dessa classe.

![classe e objeto](figuras/fig1.jpg)

## Criando uma classe em MicroPython

Antes de falar de hardware, introduza o conceito de classe e objeto com algo abstrato e simples. Vamos imaginar um classe que descreva uma pessoa, como os **atributos** (características) *nome* e *idade*, e com um **método** (uma ação conceitualmente e uma função na prática) *apresentar*.  



````python
class Pessoa:
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade

    def apresentar(self):
        print(f'Olá, meu nome é {self.nome} e tenho {self.idade} anos.')

````

Essa classe *Pessoa* apresenta uma pessoa, mas ainda **nada acontece**. Uma classe é apenas um **molde**, temos que usar esse molde para criar um **objeto**.

````python
p1 = Pessoa('Betina', 6)
p1.apresentar()
````
Com isso:
+ Criamos a classe `Pessoa` com atributos `nome` e `idade`.
+ O método `apresentar` imprime uma mensagem.
+ Criamos uma instância (`p1`) e chamamos o método `apresentar`.

E como temos o molde, podemos "criar" mais Pessoas (criar mais **instâncias**):

````python
p1 = Pessoa('Betina', 6)
p1.apresentar()
p2 = Pessoa('Maro', 5)
p2.apresentar()
````

As características iniciais (*nome* e *idade*) são definidos pela **"função mágica"** (*magic function* ou *dunder method* - *double underscores*) chamada ***\_\_init\_\_()***. O nome *init* expressa muito bem a intenção da função: **quais são os valores iniciais**.

**IMPORTANTE:** Quando criamos uma classe e instanciamos um objeto, esse objeto possui seus próprios atributos e comportamentos. O `self` garante que cada instância da classe tenha seu próprio conjunto de dados.  

Como no exemplo anterior:
+ `self.nome = nome` significa que cada objeto terá seu próprio `nome`.
+ O `self` no método `apresentar()` permite que cada objeto use seus próprios valores.
+ O `self` deve ser o primeiro parâmetro de qualquer método que interaja com os atributos da classe.
+ Por que `self` é necessário? Se não usássemos `self`, o Python não saberia a qual instância valor pertence

## Criando uma classe com a abstração de um LED
Vamos criar uma classe *Led* que abstrai as características e ações possíveis para uma Led **real**.  
+ **Atributos**: pino em que recebe o sinal e o estado dele (ligado ou desligado)
+ **Métodos**: ações *ligar*, *desligar* e *mostrar_estado* (mostrar se está aceso ou apagado)

````python
from machine import Pin

class Led:
    def __init__(self, pino):
        self.led = Pin(pino, Pin.OUT)  
        self.estado = False

    def ligar(self):
        self.led.value(1)
        self.estado = True  

    def desligar(self):
        self.led.value(0)
        self.estado = False

    def mostrar_estado(self):
        print('Aceso!' if self.estado else 'Apagado!')

led1 = Led(2)
led2 = Led(4)

led1.ligar()
led1.mostrar_estado()  
led2.mostrar_estado()
````

Notem que o método `mostrar_estado()` é uma função sem retorno, mas podemos usar implementações com **retorno**, como em funções convencionais.

````python
...
 def mostrar_estado(self):
        return 'Aceso!' if self.estado else 'Apagado!'

...
print(led1.mostrar_estado())  
print(led2.mostrar_estado()) 
````

## Implementando ações a partir de abstrações

Vamos imaginar mais uma ação que possa ser feita com um Led. Uma ação muito rotineira é a troca de estado de um Led, de aceso para apagado e vice-versa.  
Já que é uma ação comum, vamos implementar isso na classe, para poder usar sempre que for criado um objeto Led.  

````python
from machine import Pin

class Led:
    def __init__(self, pino):
        self.led = Pin(pino, Pin.OUT)
        self.estado = False

    def ligar(self):
        self.led.value(1)
        self.estado = True

    def desligar(self):
        self.led.value(0)
        self.estado = False

    def mostrar_estado(self):
        print('Aceso!' if self.estado else 'Apagado!')

    def alternar(self):
        if self.estado:
            self.desligar()
            self.mostrar_estado()
        else:
            self.ligar()
            self.mostrar_estado()
      

led1 = Led(2)
led1.alternar()  
led1.alternar() 
````

## Organizando seu código com classes

Uma maneira de organizar nosso projeto, é criar um arquivo exclusivo para a classe ou para um grupo de classes que compõem um propósito em comum.  
Podemos, por exemplo, criar um arquivo chamado `led.py` o qual terá toda a solução de classe para o Led.  

````python
from machine import Pin

class Led:
    def __init__(self, pino):
        self.led = Pin(pino, Pin.OUT)
        self.estado = False

    def ligar(self):
        self.led.value(1)
        self.estado = True

    def desligar(self):
        self.led.value(0)
        self.estado = False

    def mostrar_estado(self):
        print('Aceso!' if self.estado else 'Apagado!')

    def alternar(self):
        if self.estado:
            self.desligar()
            self.mostrar_estado()
        else:
            self.ligar()
            self.mostrar_estado()
````

E no arquivo principal (como o `main.py`), podemos importar fazendo referência ao arquivo (usando o nome que antecede o `.py`). 
````python
from led import Led
````
A referência `led` é para o arquivo que contém a classe (`led.py`) e `Led` faz referência a classe `Led` contido na arquivo. Caso tenha mais classes no arquivo, pode se importar separados por vírgulas.
