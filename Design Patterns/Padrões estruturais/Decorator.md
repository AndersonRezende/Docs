# Decorator
O Decorator é um padrão estrutural usado para adicionar funcionalidades extras a um objeto sem alterar a classe original.

## Problema


## Solução


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