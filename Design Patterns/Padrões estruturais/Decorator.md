# Decorator
O Decorator é um padrão estrutural usado para adicionar funcionalidades extras a um objeto sem alterar a classe original.

## Problema
Imagine que você tenha uma classe base para um componente de interface, como um campo de formulário. Inicialmente, você precisa de um campo simples, mas no futuro pode ser necessário que este campo tenha uma borda especial ou um tooltip de ajuda.
Utilizar herança pode resolver o problema, porém pode criar outros problemas como muitas classes combinando comportamentos diferentes. Exemplo: CampoComBorda, CampoComToolTip, CampoComBordaEToolTip, etc.

## Solução
O Decorator permite você envolver o objeto original (Componente) com wrappers (Decorator), adicionando responsabilidades em tempo de execução.

## Exemplo em código
```PHP
<?php
interface Notificacao {
    public function enviar(string $msg);
}
```

```PHP
<?php
class EmailNotificacao implements Notificacao {
    public function enviar(string $msg) {
        echo "Enviando EMAIL: $msg\n";
    }
}
```

```PHP
<?php
abstract class NotificacaoDecorator implements Notificacao {
    protected Notificacao $notificacao;

    public function __construct(Notificacao $notificacao) {
        $this->notificacao = $notificacao;
    }
}

class SmsDecorator extends NotificacaoDecorator {
    public function enviar(string $msg) {
        $this->notificacao->enviar($msg);
        echo "Enviando SMS: $msg\n";
    }
}

class LogDecorator extends NotificacaoDecorator {
    public function enviar(string $msg) {
        echo "[LOG] Enviando: $msg\n";
        $this->notificacao->enviar($msg);
    }
}
```

```PHP
<?php
$notificacao = new EmailNotificacao();

// adiciona log
$notificacao = new LogDecorator($notificacao);

// adiciona sms
$notificacao = new SmsDecorator($notificacao);

$notificacao->enviar("Olá!");

```