# Factory Method
É um padrão criacional de projeto que fornece uma interface para criar objetos em uma superclasse, mas permite que subclasses alterem o tipo de objetos que serão criados.

## Problema
Imagine uma aplicação de gerenciamento de logística. A versão inicial suporta apenas transporte com caminhões, portanto a maior parte do código ficará dentro da classe **Caminhao**.

Porém, depois de um tempo, a aplicação cresceu e passou a incorporar a logística marítma na aplicação.

O código agora possui maior parte acoplada a classe caminhão, adaptar uma classe navio para esse contexto será complicado, ainda mais se surgir outras modalidades.

Como resultado temos um código bastante sujo, repleto de condicionais que alteram o comportamento da aplicação dependendo do tipo de transporte.

## Solução
O padrão Factory Method sugere que você substitua chamadas diretas de construção de objetos (usando o operador ``new``) por chamadas para um método fábrica especial. Os objetos retornados são normalmente chamados de ``produtos``.

## Exemplo em código
```PHP
<?php
interface Veiculo
{
	public function conduzir(): void;
}
```

```PHP
<?php
class Carro implements Veiculo
{
	public function conduzir(): void
	{
		echo "Dirigindo carro";
	}
}

class Moto implements Veiculo
{
	public function conduzir(): void
	{
		echo "Pilotando moto";
	}
}
```

```PHP
<?php
abstract class VeiculoFactory
{
	abstract public function criarVeiculo(): Veiculo;

	public function iniciarVeiculo(): void
	{
		$veiculo = $this->criarVeiculo();
		$veiculo->conduzir();
	}
}
```

```PHP
<?php
class CarroFactory extends VeiculoFactory {
    public function criarVeiculo(): Veiculo {
        return new Carro();
    }
}

class MotoFactory extends VeiculoFactory {
    public function criarVeiculo(): Veiculo {
        return new Moto();
    }
}
```

```PHP
<?php
c$carroFactory = new CarroFactory();
$carroFactory->iniciarVeiculo();
// Saída: Dirigindo um carro

$motoFactory = new MotoFactory();
$motoFactory->iniciarVeiculo();
// Saída: Pilotando uma moto
```