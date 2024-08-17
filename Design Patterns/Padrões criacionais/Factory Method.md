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
```
<?php
abstract class Criador
{
	abstract public function factoryMethod(): Produto;

	public function algumaOperacao(): string
	{
		$produto = $this->factoryMethod();
		$resultado = "Criador: mesmo criado de código funcionando em " $produto->operacao();
		return $resultado;
	}
}
```

```
<?php
class CriadorConcreto1 extends Criador
{
	public function factoryMethod(): Produto
	{
		return new ProdutoConcreto1();
	}
}
```

```
<?php
class CriadorConcreto2 extends Criador
{
	public function factoryMethod(): Produto
	{
		return new ProdutoConcreto2();
	}
}
```

```
<?php
interface Produto
{
	public function operacao(): string;
}
```

```
<?php
class ProdutoConcreto1 implements Produto
{
	public function operacao(): string
	{
		return "Resultado do ProdutoConcreto1";
	}
}
```

```
<?php
class ProdutoConcreto1 implements Produto
{
	public function operacao(): string
	{
		return "Resultado do ProdutoConcreto2";
	}
}
```

```
<?php
$criador1 = new CriadorConcreto1();
echo $criador1->algumaOperacao();

$criador2 = new CriadorConcreto2();
echo $criador2->algumaOperacao();
```
