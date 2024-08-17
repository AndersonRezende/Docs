# Singleton
O Singleton é um padrão que permite você garantir que uma classe tenha apenas uma instância, enquanto provê um ponto de acesso global para essa instância.

## Problema
O padrão Singleton resolve dois problemas de uma só vez, o que viola o S (princípio da responsabilidade única) do SOLID.
<ol>
<li>Garantir que uma classe tenha apenas uma única instância. Isso pode ser útil para controlar algum recurso compartilhado, como banco de dados ou um arquivo.</li>
<li>Fornecer um ponto de acesso global para aquela instância. Pode ser útil para acessar algum objeto em qualquer lugar do código de modo seguro.</li>
</ol>

## Solução
As implementações do Singleton tem dois passos comuns:
<ol>
	<li>Fazer o construtor padrão privado para previnir que outros objetos utilizem o operador "new" nessa classe.</li>
	<li>Criar um método estático de criação que age como um construtor. Esse método chama o construtor por debaixo dos panos para criar um objeto e o salva em um campo estático. Todas as chamadas seguintes para esse método retornam esse objeto em cache.</li>
</ol>

## Exemplo em código
```
class ConexaoBD {
    private static $instancia = null;

    // Propriedade para armazenar a conexão com o banco
    private $conexao;

    // Dados para conectar ao banco
    private $host = 'localhost';
    private $usuario = 'root';
    private $senha = '';
    private $banco = 'meu_banco';

    // Construtor privado para impedir múltiplas instâncias
    private function __construct() {
        try {
            $this->conexao = new PDO("mysql:host={$this->host};dbname={$this->banco}", $this->usuario, $this->senha);
            $this->conexao->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
            echo "Conectado com sucesso ao banco de dados!\n";
        } catch (PDOException $e) {
            echo "Erro na conexão: " . $e->getMessage();
        }
    }

    // Método para obter a única instância da classe
    public static function getInstancia() {
        if (self::$instancia == null) {
            self::$instancia = new ConexaoBD();
        }
        return self::$instancia;
    }

    // Método para obter a conexão
    public function getConexao() {
        return $this->conexao;
    }

    // Impede que a instância seja clonada
    private function __clone() {}

    // Impede que a instância seja desserializada
    private function __wakeup() {}
}
```