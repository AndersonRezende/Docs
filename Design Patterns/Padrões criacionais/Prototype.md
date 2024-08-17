# Prototype
É um padrão criacional que permite copiar objetos existentes sem a necessidade de depender de suas classes.

## Problema 
Digamos que surja a necessidade de copiar uma cópia exata de um objeto, quais seriam os passos para realizar isso?
Você poderia copiar item a item, porém, muito provavelmente, alguma propriedade poderia estar privada ou oculta dentro do objeto e seria impossível realizar a cópia exata do objeto nesse caso.
Poderiamos ainda ter uma dificuldade adicional caso o objeto a ser clonado possua muitas propriedades e referências a outros objetos.

## Solução
O padrão Prototype delega o processo de clonagem para objetos reais que estão sendo clonados. O padrão declara uma interface comum para objetos que suportam a clonagem. Essa interface permite clonar um objeto sem acoplar seu código à classe desse objeto.

## Exemplo em código

```
interface Clonavel
{
	public function __clone();
}
```

```
class Proprietario
{
    public $nome;

    public function __construct($nome) {
        $this->nome = $nome;
    }

    // PHP já possuí um método mágico capaz de clonar
}
```

```
class Carro implements Clonavel
{
	private $modelo;
    private $cor;
    private $cambio;
    private $proprietario;

    public function __construct($modelo, $cor, $cambio, Proprietario $proprietario) 
    {
        $this->modelo = $modelo;
        $this->cor = $cor;
        $this->cambio = $cambio;
        $this->proprietario = $proprietario;
    }

    public function setCor($cor) 
    {
        $this->cor = $cor;
    }

    public function setProprietario(Proprietario $proprietario) 
    {
        $this->proprietario = $proprietario;
    }

    public function mostrarDetalhes() 
    {
        echo "Modelo: " . $this->modelo . "\n";
        echo "Cor: " . $this->cor . "\n";
        echo "Câmbio: " . $this->cambio . "\n";
        echo "Proprietário: " . $this->proprietario->nome . "\n";
    }

    // Implementação do método clone
    public function __clone() 
    {
    	$this->proprietario = clone $this->proprietario;
    }
}
```

```
<?php
$proprietarioOriginal = new Proprietario("João");
$carroOriginal = new Carro("Sedan", "Preto", "Automático", $proprietarioOriginal);
echo "Carro Original:\n";
$carroOriginal->mostrarDetalhes();

echo "\n";

// Clonando o carro original para criar uma nova instância
$carroClone = clone $carroOriginal;
$carroClone->setCor("Branco");
$carroClone->setProprietario(new Proprietario("Maria"));
echo "Carro Clone:\n";
$carroClone->mostrarDetalhes();

echo "\n";

// Verificando o carro original novamente
echo "Carro Original após modificação no clone:\n";
$carroOriginal->mostrarDetalhes();
```