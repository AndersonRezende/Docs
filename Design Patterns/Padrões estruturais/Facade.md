# Facade
O Facade é um padrão estrutural que fornece uma interface simples para um sistema complexo.

## Problema
Imagine que você precisa fazer seu código funcionar com um amplo conjunto de objetos que pertencem a uma sofisticada biblioteca ou framework. Normalmente, você precisaria inicializar todos aqueles objetos, rastrear as dependências, executar métodos na ordem correta, e assim por diante.

Como resultado, a lógica de negócio de suas classes vai ficar firmemente acoplada aos detalhes de implementação das classes de terceiros, tornando difícil compreendê-lo e mantê-lo.

## Solução
Uma fachada é uma classe que fornece uma interface simples para um subsistema complexo que contém muitas partes que se movem. Uma fachada pode fornecer funcionalidades limitadas em comparação com trabalhar com os subsistemas diretamente. Contudo, ela inclui apenas aquelas funcionalidades que o cliente se importa.

## Exemplo em código
```PHP
<?php
class PedidoFacade {
    private Estoque $estoque;
    private Pagamento $pagamento;
    private Notificacao $notificacao;
    private Pedido $pedido;

    public function __construct() {
        $this->estoque = new Estoque();
        $this->pagamento = new Pagamento();
        $this->notificacao = new Notificacao();
        $this->pedido = new Pedido();
    }

    public function realizarPedido($produtoId, $usuarioId, $cartao) {
        if (!$this->estoque->temProduto($produtoId)) {
            throw new Exception("Produto indisponível");
        }

        $this->pagamento->cobrar($cartao);

        $idPedido = $this->pedido->criar($produtoId, $usuarioId);

        $this->notificacao->enviarEmail($usuarioId, "Pedido #$idPedido criado com sucesso!");

        return $idPedido;
    }
}
```

```PHP
<?php
$fachada = new PedidoFacade();
$fachada->realizarPedido(10, 5, $cartao);
```