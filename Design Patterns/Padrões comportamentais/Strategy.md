# Decorator
O Strategy é um padrão comportamental que permite trocar algoritmos em tempo de execução, sem alterar o código que usa o algoritmo.

## Problema


## Solução


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