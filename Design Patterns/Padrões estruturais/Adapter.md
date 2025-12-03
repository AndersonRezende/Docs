# Adapter
É um padrão estrutural que permite objetos com interfaces incompatíveis colaborem entre si.

## Problema
Imagine que você receba dados em XML e que precise repassá-los para outro sistema em formato JSON.

## Solução
Você pode criar um adaptador. Um objeto especial que converte a interface de um objeto para que outro objeto possa entede-lo.

## Exemplo em código
```PHP
<?php
// Imagine que seu sistema espere objetos com essa assinatura
interface Pagamento
{
	public function pagar(float $valor): void;
}

// Mas você precise incluir essa nova biblioteca
class NovoPagamentoSdk
{
	public function enviarValor(float $valor): void
	{
		echo "Enviando valor $valor";
	}
}

// Seu sistema espera por pagar, mas recebe um enviarValor
```

```PHP
<?php
class NovoPagamentoSdkAdapter implements Pagamento
{
	private NovoPagamentoSdk $novoPagamentoSdk;

	public __construct(NovoPagamentoSdk $novoPagamentoSdk)
	{
		$this->novoPagamentoSdk = $novoPagamentoSdk;
	}

	public function pagar(float $valor) 
	{
        $this->novoPagamentoSdk->enviarValor($valor);
    }
}
```

```PHP
<?php
$sdk = new NovoPagamentoSdk();
$pagamento = new NovoPagamentoSdkAdapter($sdk);
$pagamento->pagar(100);
```