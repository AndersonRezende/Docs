# Builder
O Builder é um padrão de projeto criacional que permite construir objetos complexos passo a passo. Esse padrão permite produzir diferentes tipos e representações de um objeto usando o mesmo código de construção

## Problema
Imagine um objeto complexo que necessite de uma inicialização trabalhosa com muitos campos e objetos agrupados. O código de inicialização dentro do construtor fica enorme com muitos parâmetros. Agora imagine uma situação onde várias chamadas para esse construtor estejam espalhada em diversos locais do código e surja a necessidade de adicionar, remover ou modificar um parâmetro, todos os locais onde essa chamada de inicialização é feita deve ser alterado para funcionar conforme a nova alteração.
Exemplo: digamos que tenhamos um código de montadora de automóvel, cada automóvel pode ter opções de montagem específicas como: quantidade de portas, tipo de câmbio, cor, ar condicionado...
Se por acaso uma nova propriedade surgir ou deixar de existir, todos os locais onde essa construção é feita precisará ser alterada para seguir o mesmo modelo.

## Solução
O padrão Builder sugere a extração do código de construção do objeto para fora da sua própria classe e os inserir em classes "Builders". Essas classes possuem a capacidade de construir objetos específicos a partir da chamada de funções que vão estabelecendo as propriedades do objeto desejado. A parte interessante é que apenas as propriedades desejadas chamadas através de funções são adicionadas ao objeto, dessa forma não precisamos chamar um construtor enorme e passar valores como "null" ou valores genéricos para a construção do item desejado.

A solução pode ou não ter um "Diretor" que controla o processo de construção do objeto e define quais chamadas devem ser realizadas.

## Exemplo em código

### Classe Carro
Classe principal que representa o carro com suas propriedades.
```
class Carro
{
	private string $modelo;
	private int $portas;
	private string $Cambio;
	private string $cor;
	private bool $arCondicionado;

	public function setModelo(string $modelo): void {
        $this->modelo = $modelo;
    }

    public function setPortas(int $portas): void {
        $this->portas = $portas;
    }

    public function setCambio(string $cambio): void {
        $this->cambio = $cambio;
    }

    public function setCor(string $cor): void {
        $this->cor = $cor;
    }

    public function setArCondicionado(bool $arCondicionado): void {
        $this->arCondicionado = $arCondicionado;
    }

	public function mostrarDetalhes(): void {
        echo "Modelo: " . $this->modelo . "\n";
        echo "Portas: " . $this->portas . "\n";
        echo "Câmbio: " . $this->cambio . "\n";
        echo "Cor: " . $this->cor . "\n";
        echo "Ar-condicionado: " . ($this->arCondicionado ? "Sim" : "Não") . "\n";
    }
}
```

### Interface Builder
Essa interface define os métodos para configurar os diferentes atributos.
```
interface CarroBuilder
{
	public function setModelo(string $modelo): void;
    public function setPortas(int $portas): void;
    public function setCambio(string $cambio): void;
    public function setCor(string $cor): void;
    public function setArCondicionado(bool $arCondicionado): void;
    public function getCarro(): Carro;
}
```

### Implementação concreta do Builder
```
class CarroConcretoBuilder implements CarroBuilder
{
	private Carro $carro;

	public function __construct() {
        $this->carro = new Carro();
    }

    public function setModelo(string $modelo): void {
        $this->carro->setModelo($modelo);
    }

    public function setPortas(int $portas): void {
        $this->carro->setPortas($portas);
    }

    public function setCambio(string $cambio): void {
        $this->carro->setCambio($cambio);
    }

    public function setCor(string $cor): void {
        $this->carro->setCor($cor);
    }

    public function setArCondicionado(bool $arCondicionado): void {
        $this->carro->setArCondicionado($arCondicionado);
    }

    public function getCarro(): Carro {
        return $this->carro;
    }
}
```

### Classe Diretor
Classe responsável por definir quais chamadas devem ser realizadas para ficar compatível com as configurações desejadas.
```
class Diretor
{
	private CarroBuilder $builder;

	public function __construct(CarroBuilder $builder)
	{
		$this->builder = $builder;
	}

	public function construirCarroBasico(): void {
        $this->builder->setModelo("Básico");
        $this->builder->setPortas(2);
        $this->builder->setCambio("Manual");
        $this->builder->setCor("Branco");
        $this->builder->setArCondicionado(false);
    }

    public function construirCarroLuxo(): void {
        $this->builder->setModelo("Luxo");
        $this->builder->setPortas(4);
        $this->builder->setCambio("Automático");
        $this->builder->setCor("Preto");
        $this->builder->setArCondicionado(true);
    }

    public function getCarro(): Carro {
        return $this->builder->getCarro();
    }
}
```

### Utilizando
```
<?php
$builder = new CarroConcretoBuilder();
$diretor = new Diretor($builder);
$diretor->construirCarroBasico();
$carroBasico = $diretor->getCarro();
$carroBasico->mostrarDetalhes();

$builder = new CarroConcretoBuilder();
$diretor = new Diretor($builder);
$diretor->construirCarroLuxo();
$carroLuxo = $diretor->getCarro();
$carroLuxo->mostrarDetalhes();
```