# Decorator
O Strategy é um padrão comportamental que permite trocar algoritmos em tempo de execução, sem alterar o código que usa o algoritmo.

## Problema
Imagine um sistema de e-commerce onde a classe CalculadoraFrete é responsável por finalizar um pedido e precisa calcular o valor do frete. O cálculo do frete depende da transportadora que o cliente escolhe.
Se você centralizar toda a lógica dentro da classe CalculadoraFrete, o código pode crescer muito e violar o princípio do Aberto/Fechado do SOLID. Fora isso, as regras de negócio ficam intimamente ligada as regras de todas as transportadoras.

## Solução
O Strategy resolve isso externalizando a lógica do cálculo para classes separadas (Estratégias). A classe CalculadoraFrete apenas utiliza uma estratégia sem saber os detalhes do cáclulo.

## Exemplo em código
```PHP
<?php
interface FreteStrategy {
    public function calcular(float $peso): float;
}
```

```PHP
<?php
class SedexStrategy implements FreteStrategy {
    public function calcular(float $peso): float {
        return 20 + $peso * 1.5;
    }
}

class PacStrategy implements FreteStrategy {
    public function calcular(float $peso): float {
        return 10 + $peso * 1.0;
    }
}

class MotoboyStrategy implements FreteStrategy {
    public function calcular(float $peso): float {
        return 5 + $peso * 2.0;
    }
}
```

```PHP
<?php
class CalculadoraFrete {
    private FreteStrategy $strategy;

    public function __construct(FreteStrategy $strategy) {
        $this->strategy = $strategy;
    }

    public function calcular(float $peso): float {
        return $this->strategy->calcular($peso);
    }
}
```

```PHP
<?php
$calculadora = new CalculadoraFrete(new SedexStrategy());
echo $calculadora->calcular(10); // Sedex

$calculadora = new CalculadoraFrete(new PacStrategy());
echo $calculadora->calcular(10); // PAC

$calculadora = new CalculadoraFrete(new MotoboyStrategy());
echo $calculadora->calcular(10); // Motoboy
```